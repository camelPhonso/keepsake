## Abstract

The Option type is a pattern adapted into TypeScript from functional languages like Scala and Haskell. It allows us to establish an explicit contract around values that have a potential value of `null` or `undefined`.

A common challenge lies in handling values that are potentially missing. For example, a particular value may be optional in our database, meaning that a query to the data could return `null` or `undefined` instead of the expected value. This is frequently handled by defining a type of 'x' or undefined, like so:

```typescript
type userNumber = number | undefined
```

This weakens the type and now forces developers to perform an early return on all operations that require a value of type `userNumber`. That pattern is not intuitive and can easily be missed.

Using the option type would allow us to make `userNumber` stronger and enforce a more explicit contract with future developers that guarantees the proper handling of a potentially missing value.

**tags**: [[TypeScript]] [[Syntax]] [[Type Safety]]
## Example

```typescript
// our type is now a simple primitive
type userNumber = number

// some and none are defined here to expressly demonstrate the pattern at play
const some = <T>(value: T): Option<T> => ({
	_tag: "Some",
	value
})

const none: Option<neve> = ({
	_tag: "None"
})


// our bind function takes the simple primitive and returns an Option of the same type
function incrementUserNumber(givenNumber: userNumber): Option<number> {
	return !!givenNumber ? some(givenNumber + 1) : none	
}
```

By returning an `Option` type we are baking the expectation on how to handle that return value into a contract via our interfaces. That means that TypeScript itself will enforce the handling of the tags "Some" and "None".

The possibility of a missing value still needs to be handled but it is now aided by a clearer and more robust type system in our code. The caller will always be expected to explicitly handle this return type.

```typeScript
const result = incrementUserNumber(input)
if(result._tag === "None") {
	console.log("No value found")
}else{
	console.log(result.value)
}
```
A case could potentially be made that this adds complexity, however it still sets a clear pattern to be followed.