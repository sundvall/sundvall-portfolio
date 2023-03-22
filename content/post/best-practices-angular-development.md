---
title: Checklist for a maintainable angular application
description: Guides into a system
date: "2023-03-21T09:37:55+02:00"
publishDate: "2023-03-21T12:05:00+02:00"
---

As a developer in a new context, both in team and in code base, the task was to extend an existing feature. To accomplish this it was not unexpexted to spend some time on resarch and onboarding into the system of, in this case, 2000 files and 185 000 lines of code divided into 485 angular components.  
There could be numerous of ways to do this, like in person handover, documentation, well written tests and code following good standards. The balance of these factors, should all guide in the same direction, simplify the onboarding and lover the cognitive threshold as much as possible.     
Current functionality was manually tested from end to end perspective and deployed to production long time ago. In general, "it works", and until changed, it might never break, or will it? The task was to extend an existing feature, and the assumed solution was to duplicate parts of it. Was this a good idea? The existing construction revealed many magic areas, broken scopes, hard coupling, long files and lack of conventions. As a developer in this new context, it seamed very hard to understand and maintain. The onboarding was heavily dependent on great knowledge of the surrounding system and in person handover. Missing tests made refactoring a risky process.
When the architecture and conventions and common practices are weak and diverse, there is a high risk of creating a system that eventuallly breaks as it grows. Lack of rules make hacking and fixing the common pattern, and code is never deleted. No one dares to delete anything, and overrides and added 'if' conditions are common. The result might be ["a big ball of mud"](https://en.wikipedia.org/wiki/Anti-pattern#Big_ball_of_mud) - a system that most developers avoid.  

# Good habits for a maintainable system
## why quality matters
A well written system provides development in isolation, within well defined scopes. Testing is performed with a minimum of mocked data and dependencies. Frontend and backend agrees on some contracts and create their parts with different timing. Testing is done automatically and a balanced amount of well written comments guide any developer to the functionality that matters. Also, quick fixes are possible, since all maintenance from previous hotfixes are completed.

## short and long term ambitions to deliver value  
When business stress is strong for long periods, it is important to emphasize, that the technical dept can not be ignored. In short time perspective, quick solutions might be delivered, but after a while only error prone features are delivered with poor estimations. It is like maintenance of roads for traffic, or maintenance of boiler rooms (pannrum) or any complex system. In the long term perspective both development team and business share the interest to deliver features on time.  

## Requirements of a maintainable system
### what makes a system secure, performant and easy to maintain?
As a developer I in any area it ...
- should be able to rely on other parts of the system
- should be easy to onboard other developers
- shold be possible to fix, add and remove functionality without increasing of technical dept
#### onboarding
As a developer in unknown areas it ...
- should be possible to quickly grasp the main sections of the entire system
- should be possible to quickly grasp the intention of sections
- should be possible to get public api's explained
#### unit tests
As a developer I'd like to confirm, preserve and document the functionality.
- unit test documents the functionality of a feature
- unit test help to restrict the scope during development (global dependencies makes tests lengthy and hard)
- unit test increases quality of CI/CD pipeline
- unit test helps to develop quality code (single responsibility)
- unit test quickly highlites global dependencies (more mocked data)  
[Ref: "a guide to robust angular applications"](https://testing-angular.com/) 
#### integration tests
As a developer I'd like to preserve functionality during refactoring
- integration tests prove functionality of existing code during changes
- integration tests run during development helps to ship less bugs early 
- integration tests automatically performs repetetive and tedious tasks like no human can
#### inline documentation 
As a developer I'd like to spend the effort in the relevant area.
- declarative coding reduces the amount of comments
- inline documentaion balances the minimalistic style of variable names, and naming is hard
- clean code provides descriptive namings
- inline descriptions covers missing pieces
- descriptions simplifies refactoring
- descriptions simplifies usage
- descriptions helps to keep single responsbility, since a difficulty to explain may tell about not handled complexity
#### folder structure
As a developer I'd like to find relevant functionality in the general system.
- names of folders informs about features and scope
- content of folders gathers the scope
- content of folders informs about the scope
- the folderstructure depicts the architecture
#### clean code
As a developer I'd like to not concern about irrelevant information.
- tidy code makes it easy to read
- tidy code makes it easy to see deviations
- reliable abstractions decreases cognitive load
- focused code hide less bugs
- clean logs provide relevant information
#### name conventions
As a developer I'd like to reuse my knowledge about the system.
- naming makes it easy to find functionality
- naming makes it easy to predict functionality
- naming conventions decreases cognitive load
- naming conventions prepares for usage of third party tools
- naming conventions simplifies mass corrections
#### formatting
As a developer I'd like to identify functionality from visual form
- consistent formatting decreases cognitive load
- consistent formatting reveals deviations
#### conventional patterns
As a developer I'd like to reuse my knowledge from other systems.
- conventional patterns decrease cognitive load
- conventional patterns simplifies online support
- conventional patterns simplifies usage of third party tools
  
### together improve
- when developers share the general ambitions the details are more easily changed
- code review should be a positive forum
- consider the concepts of 'Dailiies' [Dogulas Chrockford](https://news.ycombinator.com/item?id=32150683)  

## based on the statements in this document the refactoring 
- antipatterns where subscription as input hides angular lifecycle update
- open css scopes introduces global scope without reason
- nested objects hiding information
- irrelevant passing around of data due to component handles too much
- impure methods manipulating objects magically
- very long files -  hard to read
- single responsibility is not respected when template, component and statemanagement has too much coupling
- linter warns for very high levels of cyclomatic complexity
- interface instead of empty class
- removal of not used variables
- and more ... documented in the git history and Pull Request
