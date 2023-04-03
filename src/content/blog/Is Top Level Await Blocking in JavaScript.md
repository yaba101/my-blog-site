---
external: false
title: 'Is Top Level Await Blocking in JavaScript?'
description: 'In JavaScript, Top Level Await is not blocking.'
date: 2023-04-02
---

In JavaScript, Top Level Await is not blocking.

Traditionally, JavaScript is a single-threaded language, meaning that it executes one line of code at a time. When it encounters an asynchronous operation, it delegates the work to a separate thread and continues executing the rest of the code. The main thread can continue executing other operations while the asynchronous operation is being completed, and once the result is ready, it delivers the result to a callback function that was specified earlier.

However, using the traditional approach of creating a callback function can lead to a phenomenon known as “callback hell,” which can make the code difficult to read, debug, and maintain. Top Level Await is a new feature introduced in ECMAScript 2019 that allows developers to write asynchronous code in a more concise and readable manner.

With Top Level Await, developers can wait for the completion of an asynchronous operation directly inside the top-level scope of a module, without the need for a callback function. This means that the main thread will be blocked until the asynchronous operation completes and returns a value.

However, it's important to note that Top Level Await doesn't block the entire program or the event loop. It only blocks the module in which it is used. This means that other modules can continue executing while the main thread is waiting for the completion of the asynchronous operation.

Here's an example to illustrate this:

```jsx
// Async function to simulate an asynchronous operation
async function fetchData() {
	const response = await fetch('<https://jsonplaceholder.typicode.com/todos/1>')
	const data = await response.json()
	return data
}

// Top Level Await
const result = await fetchData()

// Output result
console.log(result)
```

In this example, we have an async function called `fetchData()` that fetches data from a remote server using the `fetch()` function. The `await` keyword is used inside the function to wait for the response object, and then for the data object. The function returns the final data object.

We then have a Top Level Await statement that waits for the completion of the `fetchData()` function and assigns the returned value to the `result` variable. Finally, we output the `result` variable to the console.

As you can see, the main thread is blocked until the `fetchData()` function completes and returns the result. However, other modules can continue executing in the meantime.

In JavaScript, Top Level Await doesn't block the entire program or the event loop. It only blocks the module in which it is used. Other modules can continue executing while the main thread is waiting for the completion of the asynchronous operation. Here's an example that illustrates this:

```jsx
// Module 1
async function fetchData() {
	const response = await fetch('<https://jsonplaceholder.typicode.com/todos/1>')
	const data = await response.json()
	return data
}

const result = await fetchData()
console.log('Module 1:', result)

// Module 2
console.log('Module 2: This executes immediately')
```

In this example, we have two modules. `Module 1` contains an asynchronous function called `fetchData()` that fetches data from a remote server. The `await` keyword is used inside the function to wait for the response object and the data object. The function returns the final data object.

In `Module 1`, we also have a Top Level Await statement that waits for the completion of the `fetchData()` function and assigns the returned value to the `result` variable. Finally, we output the `result` variable to the console.

In `Module 2`, we have a simple `console.log()` statement that outputs a message to the console.

When we run this code, we can see that `Module 2` executes immediately, while the main thread is blocked waiting for `fetchData()` to complete in `Module 1`. Once `fetchData()` completes and returns a value, the main thread unblocks and outputs the `result` variable to the console.

This demonstrates that Top Level Await only blocks the module in which it is used, and other modules can continue executing while the main thread is waiting for the completion of the asynchronous operation.

In conclusion, Top Level Await is not blocking in JavaScript, **`but it does block the module in which it is used`**. This feature can help developers write asynchronous code in a more readable and concise manner, without the need for callback functions.
