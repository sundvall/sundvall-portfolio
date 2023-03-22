---
title: Use the tools of angular
description: Work with and not against the tools
date: "2023-03-21T10:37:55+02:00"
publishDate: "2023-03-21T12:05:00+02:00"
---
With the nose deep down in the context of an frontend application source code, it is important to remember why the framework is there and how to support its usage.
As an example, data and logic is separated and flows through inputs, outputs and services. It is possible to pass data around in other ways, but then the tool is unintentionally used.

### frontend general problems  
#### state management
- "most problems arise from the difficulty to synch state with view"
- handled by separation of logic and presentation
#### asynchronous updates
- user events
- network requests
- pending, success and error states
#### html
- browserspecificalities
- guide the user with well designed UX
- level of accessability
#### css
- the global cascade of style rules
- various screen widths
### javascript
- javascript has both "class" syntax and functions and may be developed in many ways
- javascript has many "magic" behaviours, like 
parseInt(0.5); // -> 0
parseInt(0.05); // -> 0
parseInt(0.005); // -> 0
parseInt(0.0005); // -> 0
parseInt(0.00005); // -> 0
parseInt(0.000005); // -> 0
parseInt(0.0000005); // -> 5
"1" + 2 + 3 // "123"
1 + "2" + 3 // "123"
1 + 2 + "3" // "33"
1 + 2 + 3 // 6
- see ["js is weird"](https://jsisweird.com/) for more
#### routing
- persistant data
- access levels and authorization
#### security
- injection of scripts
- validation of input
#### performance
- various bandwidths
  
### how does angular solve these problems?
#### one way data flow 
- parent child hierarchy
- dependency injection
- state is introduced by services
- state is passed to children through inputs 
- change is passed to parents with outputs
### separation of concerns
- separation of state and presentation (templates, component, service)
#### modules
- organisation of features is done by the angular modules
- modules are used to encapsulate functionality
- modules can be lazy loaded
ref ["modules - write code that is easy to delete"](https://eloquentjavascript.net/10_modules.html)
#### observables
- observables are used to handle asynchronous events
- observables encourages declarative code
#### dom updates
- angular template mixes html and logic
- the templates in most scenarios provide all functionlity to manipulate the dom
- any advanced dom-manipulation should be done with [angular tools](https://angular.io/api/core/Renderer2) to follow the angular flow 
- updates of the dom requires no additional tools
- logic inside templates impacts performance and readability
- use content projection to create reusable elements and appearance
- usre angular lifecycle methods to handle advanced updates
#### angular router
- lazy load functionality
- prohibits access
#### global and local css
- @Component...ViewEncapsulation
- styles.scsss

