## Abstract

Pure functions are one of the building blocks of Functional Programming. At their core they are functions that will **always return the same value, if the same input is passed**. This definition tries to express the idea that pure functions **do not have unintended consequences**.

This is the core concept of the functional paradigm (Statelessness), as it avoids handling state within the function. By always returning a simple value and shunning any changes to state, a pure function can be chained to other functions, used as argument and tested as a unit create more maintainable code.

**tags**: [[Functional Programming]]
## Example

```typescript
// the simplest example of a pure function returns the result of a simple mathematical operation
function addTwo(number: number):number {
	return number + 2
}

function squareValue(number: number):number {
	return number * number
}

// because they return simple types and produce no other effects they are easy to combine
const initialValue = 2

const editedValue = addTwo(squareValue(2))
```
