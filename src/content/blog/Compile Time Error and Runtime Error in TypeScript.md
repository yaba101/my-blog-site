---
external: false
title: 'Compile Time Error and Runtime Error in TypeScript'
description: 'explaining the compile time error and runtime error in typescript using examples'
date: 2023-06-07
---

TypeScript is a statically typed programming language, which means that the type of a variable is known at compile time. TypeScript code is compiled into JavaScript code before it can be executed. During this process, the TypeScript compiler checks for errors and produces an output file if there are no errors.

## Compile Time Error

A compile time error is an error that occurs during the compilation phase of the code, before the code is executed. In TypeScript, these errors are detected by the TypeScript compiler. Common examples of compile time errors in TypeScript include `syntax errors`, `type errors`, and `missing imports`.

For example, consider the following code:

```tsx
function add(a: number, b: string) {
	return a + b
}
```

This code will produce a compile time error because the parameter 'b' is of type 'string', but the '+' operator can only be used with numbers and strings.

## Runtime Error

A runtime error is an error that occurs during the execution of the code, after the code has been compiled. In TypeScript, these errors are not detected by the TypeScript compiler, but rather by the JavaScript engine at runtime. Common examples of runtime errors in TypeScript include `null reference errors`, `undefined variable errors`, and `division by zero errors`.

For example, consider the following code:

```tsx
function divide(a: number, b: number) {
	return a / b
}

divide(10, 0)
```

This code will produce a `runtime error` because we are trying to divide by zero, which is not allowed in JavaScript.

In addition, TypeScript provides features such as optional chaining and nullish coalescing, which help handle common `runtime errors` like `null reference errors`. For example, consider the following code:

```tsx
interface Person {
	name: string
	age?: number
}

const person: Person = {
	name: 'John',
}

const age = person.age ?? 'unknown'
```

In this code, we are using the nullish coalescing operator (??) to handle the case where the 'age' property of the 'person' object is undefined. This helps prevent a runtime error when trying to access the 'age' property.

## Conclusion

In summary, `compile time errors` are detected by the TypeScript compiler during the compilation phase, while `runtime errors` are detected by the JavaScript engine at runtime. It is important to understand the difference between these two types of errors, as they require different approaches to debugging and fixing.

It is important to note that preventing runtime errors is not always possible, as they often depend on user input or external factors. However, TypeScript's static typing system can help reduce the probability of runtime errors by catching them at compile time.
