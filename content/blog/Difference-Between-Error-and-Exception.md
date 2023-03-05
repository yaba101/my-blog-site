---
external: false
title: 'Difference Between Error and Exception'
description: 'Error is a type of Exception that can be caught and handled by the program. An Exception, on the other hand, is an event that occurs during the execution of a program that disrupts the normal flow of instructions.'
date: 2023-03-05
---

In the context of JavaScript, an Error is a type of Exception that can be caught and handled by the program. An Exception, on the other hand, is an event that occurs during the execution of a program that disrupts the normal flow of instructions.

## Error

An Error is a type of Exception that is thrown when a program encounters an unexpected situation. In JavaScript, Errors can be caught and handled using a try-catch block. For example, if a function is expecting a certain type of input and receives something else, it can throw an Error.

```jsx
function divide(a, b) {
	if (typeof a !== 'number' || typeof b !== 'number') {
		throw new Error('Both arguments must be numbers')
	}

	return a / b
}

try {
	console.log(divide(10, 'five'))
} catch (error) {
	console.log(error.message)
}
```

In this example, the `divide` function expects two arguments of type `number`, but if either argument is not a number, it throws an `Error`. The `try-catch` block is used to catch the `Error` and log its message to the console.

### Example 2

```jsx
const myObject = { name: 'John' }

try {
	console.log(myObject.age)
} catch (error) {
	console.log(error.message)
}
```

This example shows how an error can be thrown when a property of an object is accessed, but that property has not been defined. The `try-catch` block is used to catch the `Error` and log its message to the console.

## Exception

An Exception is an event that occurs during the execution of a program that disrupts the normal flow of instructions. In React or Node, Exceptions cannot be caught and handled using a try-catch block. Instead, they result in the program crashing or halting.

For example, if a function tries to access a variable that has not been defined, it can result in an Exception being thrown.

```jsx
console.log(x)
```

In this example, the `console.log` statement tries to access a variable `x` that has not been defined, which results in an Exception being thrown.

In summary, Errors can be caught and handled by the program, while Exceptions cannot. Therefore, it is important to handle Errors properly in order to prevent Exceptions from occurring and causing the program to crash.
