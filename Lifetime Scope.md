## Abstract

Dependency Injection can be performed with any of three different lifetime scope options on registration. These scopes essentially dictate the period for which an instance of the registered service will last.
### Transient
These services have _unique_ instances. Every new initialisation of the service will be handed a new instance, even if the service is injected multiple times within the same code module. Imagining that a method has the same transient service injected into it twice, that method will be handling two separate instances of the service. This also means that memory has been allocated two these two separate resources.

```CSharp
internal class YourController : Controller
{
	internal void YourMethod(Transient firstService, Transient secondService)
	{
		// This method has actually instantiated the same service twice
	}
}
```
### Scoped
Each instance of a scoped service will last for the duration of that request. Two modules that inject the same scoped service will have a different instance of the service each, but that instance will last for the full lifecycle of that module.

```CSharp
namespace YourProject.Controllers
{
	internal class FirstController
	{
		private readonly Scoped _context;
		internal FirstController(Scoped context)
		{
			_context = context;
		}
	}
	// Each of these controllers has a different instance of the context
	// Each instance persists for the lifecycle of the controller it was
	// injected into
	internal class SecondController
	{
		private readonly Scoped _context;
		internal SecondController(Scoped context)
		{
			_context = context;
		}
	}
}
```
### Singleton
Each instance of a singleton service lasts for the entire lifetime of the application. This should generally be applied for services with an immutable state as otherwise it's likely to lead to unintended side-effects.

```CSharp
internal class YourService
{
	private readonly Singleton _context;
	internal YourService(Singleton context)
	{
		_context = context;
		// The context will serve the same instance for every service
		// this perserves that state throughout all services that share this
		// injected context
	}
}
```

### Scope Distortion
If a service is injected into a module that has been registered with a higher lifetime scope to it's own, it will persist for the duration of the higher level module. This is a distortion of the lifetime scope of the lower level module that is implicit in the running of the program and will generally be missed by tooling.

```CSharp
// Program.cs
builder.Services.AddTransient<IService, YourService>();
builder.Services.AddScoped<IController, YourController>();
```

```CSharp
// Controller.cs
internal class YourController : Controller
{
	private readonly IService _service;
	internal YourController(IService service)
	{
		_service = service;
	}
}
```

The instance of `YourService` that has been injected into `YourController` will be persisted for the full lifecycle of `YourController`, **contrary to the behaviour dictated by it's Lifetime Scope registration**.

**tags:** [[.Net]] [[Dependency Injection]] [[Object Oriented Programming]]
## Example

```CSharp
builder.Services.AddSingleton<ISingleton, YourService>();
builder.Services.AddScoped<IScoped, YourService>();
builder.Services.AddTransient<ITransient, YourService>();

var app = builder.Build();

app.MapGet("/test", 
	(
		// always the same instance
		ISingled singletonService, 
		
		// the same instance each time an endpoint is called 
		IScoped scopedService1,
		IScoped scopedService2,

		// a new instance everytime an endpoint is called
		ITransient transientService1,
		ITransient transientService
	)
)
```

![Bald Bearded Builder's short video explaining these concepts](https://www.youtube.com/watch?v=ToFqISWq4is&ab_channel=Bald.Bearded.Builder.)