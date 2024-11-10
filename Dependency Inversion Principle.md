## Abstract
>High level modules should not depend on concrete implementations

To avoid this, we can instead move that dependency to an Interface, which means our module is now loosely coupled to a contract that can be fulfilled by any class as required instead of needing to adjust to a rigid implementation of the expected shape of its dependencies.


**tags**: [[Object Oriented Programming]] [[Design Principles]]
## Example

```csharp
public class MyController : Controller
{
	public MyController(IRepository repository)
	{
		// the declaration of IRepository as the type allows us to 
		// decouple from any set implementation of the repository
	}
}
```

```csharp
public class MyRepository : IRepository
{
	// this implementation must abide by the contract set  in IRepository
}

public class TestRepository : IRepository
{
	 // this implementation must abide by the same     contract
	 // but it can be used 
}
```