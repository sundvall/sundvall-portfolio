---
title: Tools and paradigms for development
description: Good patterns for good code in angular project
date: "2023-03-21T09:37:55+02:00"
publishDate: "2023-03-21T12:05:00+02:00"
---
A collection of very useful patterns and paradigms of angular application, collected from experience, articles and lectures.

## patterns and tools

### S in "SOLID"
The principles of "SOLID" describes patterns to create reusable and testable code. 
The 'S' is 'single responsibility principle':
- a component should do one thing
- a method should do one thing
- a function should do one thing
- naming should follow the single task
- a description of the component, function or method should reflect this fact

### "O" in "SOLID"
The "open-closed" principle. 
- use methods of imported objects to update their state
- use interface to describe the contract of imported objects
- the imported object should be closed for modification
- This principle should emphasize the importance to expose as little as possible to other components, classes, methods and functions.

### 'I' in "SOLID"
"Interface segregation"
- create small interfaces, small api's
- avoid ["banana jungle"](https://medium.com/codemonday/banana-gorilla-jungle-oop-5052b2e4d588) problem 
This principle may empasize the importance of only import and expose as few methods as possible. This principle may also emphasize the importance to import small third party dependencies.

### declarative syntax
- declarative programming is a paradigm to describe flows of actions that delegate the implementation details
- declarative programming is a paradigm to explain what to do without doing it
- declarative programming does not mutate state magically
- array methods 'map', 'reduce', 'filter' are examples of declarative functions
- 'for' and 'while' are examples of imperative functions 
- ["In multi-paradigms languages, the scale between declarative and imperative is not a clear black/white separation, but rather multiple shades of grey. It is up to us to determine which shade is the best for our projects and teams."](https://dev.to/ruizb/declarative-vs-imperative-4a7l#:~:text=Declarative%20programming%20is%20a%20paradigm,which%20mutate%20the%20program's%20state.)

### pure functions
Pure functions is set of rules to write functions without side effects. This concept extends to components and modules too. 
- a function should only act on the data it is given from its arguments
- a function should return the same result from the same input
- a function should not manipulate external state meaning avoid usage of deep nested objects or deep clone these before usage
- [more about pure functions](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-pure-function-d1c076bec976)

### flat objects
- nested objects requires more conditions to check for content
- nested ojects requires more operations to get rid of deep references
- nested objects requires more complicated mock data during testing
- nested objects obscures usage
Without pure functions and with large objects there will be an increasing need of 'if' statements to identify the state and shape of an object. 

### 'middleman' refactoring
- long chains of abstractions increase cognitive load
- no abstractions increases cognitive load
Sometimes a wish to create reusable functions adds more levels than necessary. Sometimes duplication (following same pattern) simplifies understanding.
Replace a method with its operations an use less levels of abstractions to keep actions in one place. 

### strict type checking
- confirm that all methods do return and annotate the type
Use Typescript strict.

#### restrict the scope
- confirm that the component merely acts on its provided inputs
  
#### use private scope
- only "export" when there is more than one component consuming the functionality
- use "private" as often as possible
  
#### avoid hard to read nested if's
- long chains of conditions are hard to evaluate
- assemble complex conditions in variables
- create patterns to avoid the need of complex conditions (pure functions)
- eslint has a warning for 'cyclomatic dependency'

#### remove hard coupled control of other components
- use other objects from interfaces
- do not use deeply nested properties of external objects
Encourage "louse coupling".

### style guide from angular
- assemble the components into modules
- create a shared module
- assemble services and shared models in global namespace 
  
### community standards from eslint recommended and prettier
- use the linter
- use the strict setting of typescript to create better code