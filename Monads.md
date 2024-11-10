## Abstract

Monads are a [[Design Patterns | design pattern]] that allows us to abstract complex logic behind a single function call. They can be identified by the following three characteristics:

- An **interface** that defines the return type of the function
- A **[[Pure Functions | pure function]]** that allows a value to enter the Monad ecosystem
- A **bind function** that runs transformations on the monadic values

A Monad will accept a wrapped value; unwrap it; perform the necessary changes and then return a re-wrapped value with the intended alterations. This allows us to separate the wrapping of values into their monadic shapes out onto the Monad pattern.

One example of using a Monad would be to create a log trace for function calls, as seen below.

**tags**: [[Design Patterns]] [[Abstraction]] [[Functional Programming]]  
## Example

```typescript
// interface
interface valueWithLogs<T> {
	value: T
	logs: string[]
}

// pure function
function wrapWithLogs(target: T):valueWithLogs {
	return {
		value: target,
		logs: []
	}
}

// bind function
function transformWithLogs(input: valueWithLogs, transform: (_:T)=>valueWithLogs): valueWithLogs {
	// unwrap the given value and perform transform operations on it
	const newValueWithLogs = transform(input.value)

	// rewrap the value and concatenate the new logs with the old
	return {
		value: newValueWithLogs.value,
		logs: input.logs.concat(newValueWithLogs.logs)
	}
}
```
This example isolates the complex process of concatenating the entire history of logs into a single place and allows us to perform complex transformations on the values by passing functions as an argument.

Another example of a Monad could be a function that takes in and processes and [[Option Type| option type]] 
## Resources
![how to create and use Monads](https://www.youtube.com/watch?v=C2w45qRc3aU)
This video explains the concept and defines some of the language applied in this note.