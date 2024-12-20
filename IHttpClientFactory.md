## Abstract

When an Http client is registered via the Factory it is served with a handler that is cached and maintained by the factory and applies the configurations of our instantiated client. This avoids a continuous instantiation and disposal of Http clients that could consume too many resources and lead to worsened performance.

**tags:** [[.Net]]
## Example

### Named Client
```CSharp
//Program.cs
builder.Services.AddHttpClient("ApiClient", client => 
{
	client.BaseAddress = new Uri("https://your-url.com");
});

// YourController.cs
internal class YourController : Controller
{
	private readonly HttpClient _client;

	internal YourController(HttpClient client)
	{
		_client = client;
	}
}
```

Alternatively, I have also successfully applied the following and bypassed injection in the Controller class Constructor:
```CSharp
// Program.cs
builder.Services.AddScoped(serviceProvider =>
{
	serviceProvider
		.GetRequiredService<IHttpClientFactory>()
		.CreateClient("ApiClient");
})
```

### Typed Client
```CSharp
// Program.cs
builder.Services.AddHttpClient<IHttpClientFactory, ApiClient>(client =>
{
	client.BaseAddress = new Uri("https://your-url.com");
});

// YourController.cs
internal class YourController : Controller
{
	private readonly HttpClient _client;

	internal YourController(HttpClient client)
	{
		_client = client;
	}
}
```