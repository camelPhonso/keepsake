## Abstract

Boxing and, subsequently, unboxing are concepts in statically-typed languages like C#. These are not patterns or methods generally used as techniques when coding, but concepts to explain a side effect of the way the language works.

In the types system of C# every value can be treated as an object. This leads to a specific situation where our handling of values can lead to them being cast into objects. In other words, a variable of time `int` for example can be cast into an `object`.

Similarly to [[Type Inference|type inference]] in JavaScript, this leads to that variable being treated now as it's 'new value'. In order to deal with that variable as an integer we must now unbox it.

- **Boxing** is implicit
- **Unboxing** is explicit

Both of these processes come with a memory and processing time cost.

**tags**: [[C#]] 
## Example

```csharp
class Program
{
	static void Main()
	{
		int someNumber = 269;
		// implicit boxing
		object someNumberAsObject = someNumber;
		// explicit unboxing
		int numberAsNumberAgain = (int)someNumberAsObject;
	}
}
```

Although boxing will cause a slight deficit in performance (which could compound) and adds strain to the [[Garbage Collection | garbage collector]] it's more relevant to understand the cases where it can happen in an abstracted process.

```csharp
class Program
{
	static void Main()
	{
		int someNumber = 269;
		var allNumbers = Enumerable.Range(269, 1312).ToArray();
		// ArrayList will box each value in the background
		var arrayList = new ArrayList(allNumbers);
		// List stores a single array without boxing its values
		var list = new List<int>(allNumbers);
	}
}
```
In this example, the simple choice between an ArrayList or a generic List will determine whether our program boxes and unboxes the values within the array.