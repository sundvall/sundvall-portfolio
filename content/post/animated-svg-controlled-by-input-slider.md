---
title: Animated svg controlled by input slider
description: Svg markup with animations connected to form input slider
date: "2019-05-03T09:37:55+02:00"
publishDate: "2022-11-18T12:05:00+02:00"
---

Crossbrowser svg animation created in raw svg markup inside angular component
{{< figure src="/post/images/animated-svg-connected-to-slider-480.gif" caption="Screenshot from the animated svg connected to a slider" >}}
<!--more-->

A decent amount of tweeks were made to make the animation work both in Safari and Chrome browsers.

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