---
external: false
title: "Why We Don't Use React Hooks Inside Loops or Conditionally?"
description: 'One of the important rules when using Hooks is to never use them inside loops or conditions.'
date: 2023-03-30
---

**One of the important rules when using Hooks is to never use them inside loops or conditions.**

But why? In this blog, I will try to give concrete examples and why we don’t use hooks inside conditions. I’m gonna give you two reasons. one of them is pretty much solid which is related to the implementation of hooks.

When React renders a component, it runs all of the code inside it, which includes any loops or conditions. When a Hook is called inside a loop or condition, it's called multiple times, which can lead to unexpected behavior and performance issues.

For example, let's say we have a simple component that `renders` a list of items. We want to use the `useState` Hook to keep track of whether each item has been clicked or not. In the first example, we call the `useState` Hook outside of the loop, which allows us to keep track of the state of each item as expected:

```jsx
import React, { useState } from 'react'

function ItemList({ items }) {
	const [clickedItems, setClickedItems] = useState({})

	return (
		<ul>
			{items.map((item) => {
				const isClicked = clickedItems[item.id]

				return (
					<li
						key={item.id}
						onClick={() =>
							setClickedItems({ ...clickedItems, [item.id]: true })
						}>
						{item.name}
						{isClicked && <span> (clicked)</span>}
					</li>
				)
			})}
		</ul>
	)
}
```

However, if we move the `useState` call inside the loop, we have a problem. Every time the loop runs, a new state variable `isClicked` is created. When the user clicks on an item, only the last `isClicked` variable is updated, which means that only the last item in the list will show as clicked, regardless of which item was actually clicked:

```jsx
import React from 'react'

function ItemList({ items }) {
	return (
		<ul>
			{items.map((item) => {
				const [isClicked, setIsClicked] = useState(false)

				return (
					<li key={item.id} onClick={() => setIsClicked(true)}>
						{item.name}
						{isClicked && <span> (clicked)</span>}
					</li>
				)
			})}
		</ul>
	)
}
```

To avoid this issue, we should always call Hooks at the top level of the component, and outside of any loops or conditions. This ensures that Hooks are only called once per component render, and that the state is updated correctly.

The other reason is related to the implementation of the hook. Before I talk about I have to give credit for the next reason to \***\*Shawn @swyx Wang, he talked about it at `JSConf Asia 2018` , which I genuinely copied and summarized his talk 😊. I highly recommend watching it. Here is the [Link](https://www.youtube.com/watch?v=KJP1E-Y-xyo&list=WL&index=19&t=910s)**

### The second reason 👇

First thing is first, Did you know that hooks are stored in Array?

for example

```jsx
import { useState } from 'react'
function App() {
	const [username, setUserName] = useState(null) // This is index 0
	const [password, setPassword] = useState(null) // This is index 1
}
```

that’s how hooks are stored beside the scene, let’s explore the implementation of hook

`TL;DR` **this is the overall implementation**

```jsx
const React = (function () {
	let hooks = []
	let index = 0

	function useState(initVal) {
		const state = hooks[index] || initVal
		const _index = index
		const setState = (newVal) => (hooks[_index] = newVal)
		index++
		return [state, setState]
	}

	function render(Component) {
		index = 0
		const C = Component()
		C.render()
		return C
	}

	return { useState, render }
})()

function Component() {
	const [count, setCount] = React.useState(1)
	const [text, setText] = React.useState('Array')

	return {
		render: () => console.log({ count, text }),
		click: () => setCount(count + 1),
		type: (word) => setText(word),
	}
}
```

**let’s walk through the code**

```jsx
// assume this is 'react' which you import hooks
// import {useState} from 'react'

const React = (function () {
	let hooks = [] // store for our states
	let index = 0 // to loop through the hooks array

	function useState(initVal) {
		const state = hooks[index] || initVal // this the state
		const _index = index // reassign it to private state ( closure ), because w
		const setState = (newVal) => (hooks[_index] = newVal)
		index++ // we are increment to go to for the next state,if we have!
		return [state, setState]
	}

	function render(Component) {
		// take fn as a parameter
		index = 0 // reseting index to zero at rerendering to start from the first index in hooks array
		const C = Component()
		C.render() // call render function to rerender when the state changes
		return C
	}

	return { useState, render } // exposing useState hook and render method so that
	// we can access it later ( React.useState(value | null ) or React.render(fn)
})()
```

**key points from the above code snippet**

- hooks are stored in an array
- when the state changes we have to run `rerender` to see the effect

**Now the other part**

```jsx
// This is React component
// you can even tell from the naming of the function ( capitalized )😊
function App() {
	// initialize two separate states
	const [count, setCount] = React.useState(1)
	const [text, setText] = React.useState('Array')

	return {
		// instead of returning JSX code let's see using normal javascript code
		// these are methods that log and update states

		render: () => console.log({ count, text }),
		click: () => setCount(count + 1),
		type: (word) => setText(word),
	}
}
```

**next let’s see what will happen when we call the methods.**

```jsx
React.render(App) // Output: { count: 1, text: 'Array' }
// what happens when we call render from React we are accessing the default value that we assign in the App component
// let's assign it to the variable we call it firstRender
let firstRender = React.render(App)
firstRender.click() // Output : { count: 1, text: 'Array' }
// Yeah, they are the same. Do you know why??
// It's because we have to rerender the component to see the change
// so let's call it
let secondRender = React.render(App) // Output: { count: 1, text: 'Array' }
// { count: 2, text: 'Array' }
// this time let's add another state value to the text state
secondRender.type('Object') // the output will be the same like secondRender
React.render() // Output: { count: 1, text: 'Array' }
//  { count: 2, text: 'Array' }
//  { count: 2, text: 'Object' }
```

Here is the gotcha, what will happen if I put one of App component state in `if statement`??

```jsx
function App() {
	const randomNumber = Math.random()
	console.log(randomNumber) // to track the output number
	if (randomNumber < 0.6) {
		var [count, setCount] = React.useState(1) // I used var in these case because of block scope rule
	}
	const [text, setText] = React.useState('Array')

	return {
		render: () => console.log({ count, text }),
		click: () => setCount(count + 1),
		type: (word) => setText(word),
	}
}
```

**what do you think will happen if you call `App component` like this??**

```jsx
let firstRender = React.render(App)
firstRender.click()
let secondRender = React.render(App)
secondRender.type('Object')
React.render()
```

**The output will be like this:**

```jsx
// Case I
// I don't have to explain the output they're self explanatory 🤷‍♂️
0.4982180653101771
{ count: 1, text: 'Array' }
0.9504392668945025
{ count: undefined, text: 2 }
0.5825334907938822
{ count: 'hello', text: 'Array' }

//////////////////////////////////
// Case II
0.2614350323665702
{ count: 1, text: 'Array' }
0.5075312455920216
{ count: 2, text: 'Array' }
0.030347558095424665
{ count: 2, text: 'hello' }
/////////////////////////////////

// Case III
0.9045373782712329
{ count: undefined, text: 'Array' }
hooks.js:33
        click: () => setCount(count + 1),
                     ^
TypeError: setCount is not a function

//////////////////////////////////////
// Case IV
0.5228505836761614
{ count: 1, text: 'Array' }
0.10014735916156714
{ count: 2, text: 'Array' }
0.783435741886424
{ count: undefined, text: 2 }
////////////////////////////////
// Case V
0.3772350437577019
{ count: 1, text: 'Array' }
0.7562871990096205
{ count: undefined, text: 2 }
0.6187987622828977
{ count: undefined, text: 'hello' }
```

_I think this is it 😍!!
Thanks for reading so far......_
