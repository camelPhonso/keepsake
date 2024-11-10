## Abstract

Recursive functions are pure functions that call themselves until their base case is true. The recursive function will return the combined outcome of all it's calls. The make of a recursive function is defined by the following three parts: 

- **base case** - an evaluation that must return true before the function stops calling itself
- **function declaration** - the body of the function which should be [[Pure Functions | pure]]
- **recursive function call** - which triggers a new run of the function and passes it an updated copy of the values to be processed

Recursive functions are functional pattern that allows developers to loop through data or even, frequently, to process nested data such as multi-level objects or arrays of arrays.

This pattern avoids data mutability by [[value vs reference | passing variables by value]] and it shuns state by recursively calling a [[Pure Functions | pure function]]

**tags**: [[Functional Programming]] [[Design Patterns]]
## Example

```typescript
function updateNestedObject(
	currentValue: NestedObject,
	newValue: Partial<NestedObject>,
	index: number,
	changes: Partial<NestedObject>
){
	//base case
	if(index === Object.keys(currentValue).length) {
		return {...currentValue, ...changes}
	}
	const currentKey = Object.keys(currentValue)[index]

	// function declaration
	switch(true) {
		case !newValue[currentKey] && newValue[currentKey] !== "object":
			changes = {
				...changes,
				[currentKey]: currentValue[currentKey]
			}
			break
		case typeof currentValue[currentKey] === "object":
			// in this case the function also calls itself on each nesting level
			changes = {
				...changes,
				[currentKey]: updateNestedObject(
					currentValue[currentKey],
					newValue,
					0,
					{}
				)
			}
			break
		case !!currentValue[currentKey] && !!newValue[currentKey]:
			changes = {
				...changes,
				[currentKey]: newValue[currentKey]
			}
			break
		default:
			changes = {
				...changes,
				[currentKey]: newValue[currentKey]
			}
	}

	// recursive function call
	const newChanges = {...changes}
	return updateStorage(currentValue, newValue, index + 1, newChanges)
}
```
The function above allows us to pass a nested object, along with our intended changes to only parts of that object. This is aided by a strong [[Type Systems | type system]] in our code.

The function passes a reference of the changes made onto it's next call and performs the changes necessary to the newest copy with each call. If any property of the object is a new object then the function will call itself on that new level.

Once every property of the object has been verified the function will return a new object that includes all changes made.

By performing changes to a new copy every time this function protects data immutability, avoiding unintended consequences to our logic.