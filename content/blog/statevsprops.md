---
external: false
title: 'State Vs Props in React'
description: 'Props and state are different, but they work together, also the flow of data in react.'
date: 2023-02-24
---

## State

Let's start with State first.The state is a built-in React object that is used to contain data or information about the component. A component's state can change over time; whenever it changes, the component re-renders.
It should have any initial value (`string, number, boolean, null, object or array`).
Only the component that owns state can change it.
State is private too.

```js
import { useState } from 'react'
const App = () => {
	const [value, setValue] = useState(0) // value is current state
	const handler = () => {
		setValue(
			(prev) => prev + 1
		) /* using prev parameter we can access the previous value
		which is a best practice when dealing with this kind of situation where the current value is calculated based on the previous state.*/
	}
	return (
		<div>
			<button onClick={handler}>Add One</button>{' '}
			{/* updating the current value by one*/}
		</div>
	)
}
export default App
```

## Props

Props are like arguments you pass to a function. They let a parent component pass data to a child component and customize its appearance.

```js
import { useState } from 'react'
const App = () => {
	const [value, setValue] = useState(0) // value is current state
	const handler = () => {
		setValue((prev) => prev + 1)
	}
	return (
		<div>
			<h1> The value is: {value}</h1>
			<button onClick={handler}>Add One</button>
			<Display value={value} />
			{/* value is passed to the Display component as a prop */}
		</div>
	)
}

const Display = ({ value }) => {
	return <div>The value that comes from parent component is {value}</div>
}
export default App
```

`Props` and `state` are different, but they work together.
A `parent component` will often keep some information in `state` (so that it can change it), and pass it down to `child components` as `their props`.

However, props are immutable—a term from computer science meaning “unchangeable.”
When a component needs to change its props,For example we cannot update the value of number inside the Display component directly, it will have to “ask” its parent component to pass it different props—a new object!

`"Don’t try to change props directly from child component”.`

### why???

Because in react, data can only flow from top-to-bottom ( parent to child component).

BUT, there is a way to change the state of parent from child component through callback/function props.
let's see an example of that.

```js
import { useState } from 'react'

const Parent = () => {
	const [message, setMessage] = useState('This is Parent Component')

	const changeMessage = (message) => {
		setMessage(message)
	}

	return (
		<div>
			<h1> {message}</h1>
			<Child changeMessage={changeMessage} />
		</div>
	)
}

const Child = ({ changeMessage }) => {
	return (
		<div>
			<h1> Child Component</h1>
			<button onClick={() => changeMessage('This is Child Component')}>
				Change it
			</button>
		</div>
	)
}

export default Parent
```

#### So that Props are immutable, State also should be treated as immutable for many reasons.

`As rule of thumbs: Treat all state in React as immutable (read-only).`

# Recap

| State                                                                                            | Props                                                              |
| ------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------ |
| **`Data or information about the component`**                                                    | **`arguments you pass to a function/components`**                  |
| **`When the state changes the component re-renders.`**                                           | **`it is way of parent component pass data to a child component`** |
| **`Only the component that owns state can change it.`**                                          | **`cannot change props directly`**                                 |
| **`State should be treated as immuatable even if they are mutable in case of object and array`** | **`they are immutable`**                                           |
