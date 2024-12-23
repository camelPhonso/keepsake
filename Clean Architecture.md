## Abstract

A suggested structure for projects that is generally implemented alongside [[Domain Driven Design ]] - though [they are different concepts](https://www.youtube.com/watch?v=eUW2CYAT1Nk&ab_channel=AmichaiMantinband) - and aims to help developers avoid Boundary Crossing by visually representing the various concern areas of a product with different Projects within the same Solution.

Applying _clean architecture_ facilitates the maintainability of a code base by enforcing a clear separation of concerns that allows for different parts of your code to be updated, refactored or entirely swapped out over time with minimal disruption to the rest of the code base.

The simplified setup outlines 4 Projects created within the Solution defined as follows:

![Clean Architecture Diagram](https://www.ssw.com.au/rules/static/6b8c75864933e5265075d7ae7f90b165/d7542/ca-diagram.png)

### Domain
Relates to the enterprise-level domain of the codebase. This will typically include, for example, Entity/Model definitions (for projects where EntityframeworkCore is used)
### Infrastructure
Defines the specific implementations of the contracts set in the Infrastructure project
### Application
Holds the contracts for how the code will interact with the modules defined in the Domain project. This project is dedicated exclusively for **abstractions**
### Presentation
Also sometimes called _WebUI_, this project defines the client-facing layer that will consume the services rendered by the Application project. This project also references the Infrastructure project but solely for the purpose of registration of services in `Program.cs`

**tags:** [[Architecture]] [[Design Principles]] [[.Net]] 
## Example

![Nick Chapsas detailed video explaining Clean Architecture](https://www.youtube.com/watch?v=YiVqwoFMieg&ab_channel=NickChapsas)

[Example repo and CLI template for a Clean Architecture implementation](https://github.com/jasontaylordev/CleanArchitecture)
