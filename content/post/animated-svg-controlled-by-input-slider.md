---
title: Animated svg controlled by input slider
description: Svg markup with animations connected to form input slider
date: "2019-05-03T09:37:55+02:00"
publishDate: "2022-11-18T12:05:00+02:00"
---

Crossbrowser svg animation created in raw svg markup inside angular component
<a href="/sundvall-portfolio/post/animated-svg-controlled-by-input-slider/" >

{{< figure src="/sundvall-portfolio/post/images/animated-svg-connected-to-slider-480.gif" caption="Screenshot of animated svg controlled by custom styled input range form element." >}}</a>
<!--more-->

A large amount of tweeks were made to make the animation work both in Safari and Chrome browsers.
Safari turned out to be very strict about the implementation of parameters controlling the movement.

The working example may be seen on [Vattenfall  webpage](https://www.vattenfall.se/elavtal/teckna-elavtal/)

Svg may be animated by the animateTransform element. It animates its parent element, as here a group.


```svg
<g id="flyer">
    <animateTransform #angularspecific-template-reference 
        id="animflyersmalltomedium" 
        attributeName="transform" 
        attributeType="XML"
        type="translate" calcMode="spline" 
        dur="500ms" repeatCount="1" fill="freeze"
        from="90,0" 
        to="0,0" 
        keyTimes="0; 0.5; 0.75; 1"
        keySplines="0.44 0.78 0.44 0.78; 0.44 0.78 0.44 0.78; 0.44 0.78 0.44 0.78" 
        additive="replace" begin="indefinite"/>
```


The animation is then triggered from javascript like this:
```javascript
document.querySelector("#animflyersmalltomedium").beginElement();
```



**markup vs js-library**
 The benefit from doing the animation in this markup style is the simplicity. All code required for the animation is in the markup and no external dependencies are required. It is simple to test locally in a plain oldschool html document.
 All resources for markup are found att [MDN](https://developer.mozilla.org/en-US/docs/Web/SVG)


**The drawback** is also exactly this. The document quickly grows, and it is important to get the syntax exactly correct to get it working in safari browser. A javascript-library should facilitate the crossbrowser issues and also provide all the benefits of encapsulate and abstract away the verbose syntax. 
More guides to javascript libraries for svg animation are found at [Frontend horse] (https://frontend.horse/)


**The input range is style like this to work cross browser**
```css
input[type="range"] {
  -webkit-appearance: none;
  background: rgba(0, 0, 128, 0.3);
  border-radius: 5px;
  background-image: linear-gradient(#1e324f, #1e324f);
  background-repeat: no-repeat;
}
input[type="range"]::-webkit-slider-thumb {
  -webkit-appearance: none;
  margin-bottom: 0.5rem;
  height: 2.5rem;
  width: 2.5rem;
  border-radius: 50%;
  background: white;
  border: #1e324f 3px solid;
  cursor: pointer;
  box-shadow: 0 0 2px 0 black;
  transition: background 0.3s ease-in-out;
}
input[type="range"]:focus::-webkit-slider-thumb {
  outline-style: double;
  outline-color: #2071b5;
  outline-width: thick;
}
```
See an example here:
<p class="codepen" data-height="300" data-default-tab="html,result" data-slug-hash="eYKVjdR" data-user="Sun-D-vall" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/Sun-D-vall/pen/eYKVjdR">
  custom-style input range</a> by Martin (<a href="https://codepen.io/Sun-D-vall">@Sun-D-vall</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>