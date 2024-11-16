## Abstract

Higher Order Components are a pattern in React that favours [[Composition]] over [[Inheritance]] by allowing developers to combine different components to implement complex functionality without the need for prop-drilling. This keeps the different components de-coupled, making maintenance of the code easier.

The name refers to these components being declared as higher order functions - meaning they can take a function (or component) as a component and return a new function (or component) that allows for more functionality without altering the one passed in as an argument.

Common examples of Higher Order Components wrap other components to do any of the following:
- Adding Auth logic
- Implementing error-handling
- Managing and communicating to the user a `loading` state

**tags**: [[Functional Programming]] [[Pure Functions]] [[Dependency Inversion Principle]]
## Example

```typescript
export default function WithLoginPrompt(BaseComponent) {
	function WrappedComponent(props){
		const isLoggedIn = useContext()
		const [isShowingPrompt, setIsShowingPrompt] = useState(false)

		return (
			<>
				{isShowingPrompt ? (
					<button
						onClick={()=>setIsShowingPrompt(false)}
					>
						Log in to use this feature
					</button>
				) : (
					<>
						{isLoggedIn ? (
							<BaseComponent {...props}/>
						) : (
							<button
								onClick={()=> setIsShowingPrompt(true)}
							>
								<BaseComponent 
									{...props}
									disabled={true} 
								/>
							</button>
						)}
					</>
				)}
			</>
		)
	}

	return WrappedComponent
}
```

```TypeScript
import WithLogInPrompt
import SimpleButton

export default OtherComponent(){
	const ButtonWithLoginPrompt = WithLoginPrompt(SimpleButton)

	// this component can now receive the props for SimpleButton
	return <ButtonWithLoginPrompt />
}
```
`WithLoginPrompt` will wrap a base component and render it naturally `if` the user is logged in. Otherwise, the logic wrapping that component will render a prompt telling the user to login when they click the original, simpler component (this example assumes we are wrapping a button, but to make the HOC generic we would avoid calling the `disabled` attribute)

Now, the two components are decoupled but form a more complex feature. The same logic to prompt a login can be applied in other parts of our app.

Although this is typically a pattern of Functional programming, it also helps us abide by the [[Dependency Inversion Principle|principle of dependency inversion]].

To preserve the existing Types System we can declare the Higher Order Component with the use of generic Types:

```TypeScript
export default function HigherOrderComponent<T>(
	// ComponentType is imported from React
	BaseComponent: ComponentType<T>
) {
	return function WrappedComponent(props: T) {
		// return the new component tsx here
	}
}
```