## Abstract
Whenever a code module is coupled to another this is dependency injection. There are three types of dependency injection: 
- Constructor Injection
- Setter Injection
- Interface Injection

However, colloquially, Dependency Injection is used to refer to any instance in which Interface Injection is used in order to observe the [[Dependency Inversion Principle]]. A common design pattern by means of which we can decouple, or at least loosely couple two modules.

**tags**: [[Object Oriented Programming]] [[Design Patterns]]
## Example

### Constructor Injection
```csharp
public class HigherModule(ClientType client)
{
	private ClientType _client = client;
}
```

### Setter Injection
```csharp
public class HigherModule
{
	private ClientType _client;
	
	public void SetClient(ClientType client)
	{
		_client = client;
	}
}
```

### Interface Injection
```csharp
public class HigherModule : IHigherModule
{
	private ClientType _client;

	public HigherModule(ClientType client)
	{
		_client = client;
	}
}

public Interface IHigherModule
{
	private Client client;
	
	void SetClient(Client client)
	{
		_client = client;
	}
}
```
