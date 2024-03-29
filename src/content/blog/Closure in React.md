---
external: false
title: 'Closure in React'
description: "a closure is formed when an inner function is returned from an outer function, allowing the inner function to access the outer function's variables and parameters even after the outer function has completed execution."
date: 2023-03-07
---

In JavaScript, a closure is formed when an inner function is returned from an outer function, allowing the inner function to access the outer function's variables and parameters even after the outer function has completed execution. Closures are widely used in React to manage state and implement event handlers.

Here's a simple example of closure in React:

```jsx
import { useState } from 'react'

function Counter() {
	const [count, setCount] = useState(0)

	function handleClick() {
		setCount(count + 1)
	}

	return (
		<div>
			<p>You clicked {count} times</p>
			<button onClick={handleClick}>Click me</button>
		</div>
	)
}
```

In this example, `useState` is a hook that returns an array with two elements: the current state value, and a function to update it. The `count` variable and the `setCount` function are declared using destructuring assignment.

The `handleClick` function is defined inside the `Counter` function, and it has access to the `count` and `setCount` variables through closure. When the user clicks the button, the `handleClick` function is called, and it updates the state by calling `setCount` with the new value of `count`.

Using closure in this way allows us to manage states in a functional component without having to use class components or global variables. It's a powerful tool for building complex UIs in React.

Another use case of closure in React is for creating reusable components with private states.

For example, let's say you want to create a component that displays a list of items and allows the user to add and remove items from the list. You could use closure to keep track of the list of items internally, without exposing it to the outside world.

```jsx
// App.js

import { useState } from 'react'
import AddItemForm from './AddItemForm'

function List() {
	const [items, setItems] = useState([]) // private states

	// simply setter function, this can be passed as prop into another component for modifying internal state without exposing the internal state values
	function addItem(item) {
		setItems([...items, item])
	}

	function removeItem(index) {
		// we don't need the item that's why we add _ in the filter first parameter.
		setItems(items.filter((_, i) => i !== index))
	}
	return (
		<div>
			<ul>
				{items.map((item, index) => (
					<li key={index}>
						{item} <button onClick={() => removeItem(index)}>Remove</button>
					</li>
				))}
			</ul>
			<AddItemForm onAdd={addItem} />
		</div>
	)
}
```

#

```jsx
// AddItemForm.js
function AddItemForm({ onAdd }) {
	const [value, setValue] = useState('')

	function handleSubmit(event) {
		event.preventDefault()
		onAdd(value)
		setValue('')
	}

	return (
		<form onSubmit={handleSubmit}>
			<input
				type='text'
				value={value}
				onChange={(e) => setValue(e.target.value)}
			/>
			<button type='submit'>Add</button>
		</form>
	)
}
```

In this example, the `List` component declares a state variable `item` using `useState`, and defines two functions `addItem` and `removeItem` to modify the state. These functions are passed down to the `AddItemForm` component as props.

The `AddItemForm` component also uses `useState` to declare a state variable `value`, which represents the current value of the input field. When the form is submitted, the `handleSubmit` function is called, which adds the new item to the list by calling `onAdd` with the current value of `value`.

Because the `items`, `addItem`, and `removeItem` variables are declared inside the `List` component, they are not visible or modifiable from the outside. They are only accessible to the `List` component and its children, thanks to closure. This makes it easy to reuse the `List` component in other parts of your application, without worrying about name collisions or unintended side effects.

<aside>
💡 Actually, there is another best example of closure in react that demonstrate well,             Did you wonder about the implementation of useState in react.

</aside>

let’s see the implementation real quick

```jsx
function useState(initialValue) {
	let value = initialValue
	const state = value
	const setState = (newValue) => (value = newValue)
	return [state, setState]
}
const [message, setMessage] = useState('Hello')
console.log(message) // ??
setMessage('World')
console.log(message) // ??
```

when we run this code the result will be

```jsx
const [message, setMessage] = useState('Hello')
console.log(message) // Hello
setMessage('World')
console.log(message) // Hello
```

`wait what? what happened?`

That wasn’t the correct implementation even if I said that was the correct one, sorry but

here is the catch, or if you spotted it good for you, you have understood what closure means.

let me show you where we got wrong when we implemented the useState or you can say bug

```jsx
function useState(initialValue){
	let value = initialValue
	-- const state = value  ❌
	++ const state = () => value ✅ // making this as getter function
	const setState = (newValue) => value = newValue // this is actual setter function
	return [state,setState]
}
const [message,setMessage] = useState('Hello')
console.log(message()) // Hello, we calling message cuz it's a getter function now
setMessage('World')
console.log(message()) // World
```

The problem was the variable `value` is a private state/value. we are mutating the variable `value` using `setMessage` but we want to access the newly passed value through `setMessage` so we have to use closure to access the internal state/value, in this case, the variable `value` using variable `state` in the useState function.

In conclusion, Closures are a fundamental concept in JavaScript that can be used in react in many different ways. A private variable is one example of closure.
