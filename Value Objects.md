## Abstract

This design pattern allows us to enforce Type safety by replacing primitive types. The implementation of Value Objects significantly increases the complexity of the code, not only by the extensive setup it demands but also by adding complexity to any parts of the code that employ these objects.

The trade-off in implementing Value Objects is in fighting [[Primitive Obsession]]. 

The main characteristics of Value Objects are as follows:
- **they are defined by their value** - two objects with the same values are considered equal
- **they are immutable by design** - once created, a Value Object cannot be altered


**tags:** [[Design Patterns]] [[Object Oriented Programming]] [[Type Safety]]
## Example

```CSharp
public struct User(string Name, string Email);

var validUser = new("fake@email.com", "Jane Doe");

var alsoValidUser = new("Jane Doe", "Jane Doe");

var anotherValidUser = new("Jane Doe", "perfectly acceptable email address");
```

Primitive types do not verify data equality, meaning that that we could pass the same string for two different arguments of the type.

Equally, primitive types do not impose reasonable constraints that our use case may expect. For example, not all strings are a valid email.

[Implementing Value Objects](https://learn.microsoft.com/en-us/dotnet/architecture/microservices/microservice-ddd-cqrs-patterns/implement-value-objects) allows us to strengthen our [[Types System]] by introducing all these constraints.

```CSharp
public struct User(UserName Name, UserEmail Email);

UserName userName = new("Jane Doe");
UserEmail userEmail = new("jane@email.com");

var validUser = new(userName, userEmail);

var invalidUser = new(userEmail, userName) // throws invalid argument exception
``