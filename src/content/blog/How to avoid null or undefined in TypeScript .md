---
external: false
title: 'How to avoid null or undefined in TypeScript'
description: 'How to avoid passing null or undefined in type level in TypeScript'
date: 2023-03-23
---

#

I recently wanted to only accept anything but not null or undefined, and I want typescript to yell at me or throw an error if I pass null or undefined into the code at type level.

I know that I could specify that manually but in some cases, that is not the solution so there is a clever way of doing that, which is using `Object` or `{}`.

I could give example but its TypeScript-specific.

let’s say I wanted to accept anything but not null and undefined using generics.

```tsx
type SomethingThatIsNotUndefinedOrNull<
	T extends string | number | boolean | string[] | number[] | boolean[]
> = {
	value: T
} // these are some of them, imagine if I want all types 😨
type SomeType = SomethingThatIsNotUndefinedOrNull<string> // It will work
type ItWillYellAtYou = SomethingThatIsNotUndefinedOrNull<null | undefined>
```

So what the solution, Here is the savior comes in ⇒ `Object` or `{}`

```tsx
type SomethingThatIsNotUndefinedOrNull<T extends Object> = {
	value: T
} // Or {} instead of Object
type SomeType = SomethingThatIsNotUndefinedOrNull<string> // It will work
type ItWillYellAtYou = SomethingThatIsNotUndefinedOrNull<null | undefined>
```

### But why does its work??

### **Here is the higher level of explanation:**

Everything that has methods are object, simply everything in JavaScript is an object because every primitive type in JavaScript have an object Wrapper behind the scene, but null and undefined don’t have; that means they don’t have a method on them.

- **Optional: Deep Explanation**
  All data types in JavaScript must fall into one of six primitive types, or the object type. Primitive types include `boolean`, `null`, `undefined`, `string`, `number` and `symbol`; **everything that is not primitive is an object**.
  Under the hood, primitive values have *object wrappers*. When you try to access the `length` property on the string literal, JavaScript creates a temporary `object wrapper` around the primitive and access the length property of that object wrapper. After the property has been retrieved, the object wrapper is discarded. This is known as `autoboxing.`
  For Example:
  ```jsx
  const foo = 'bar'
  // When you run `foo.length`, it's similar to this which is what's going on behind  the scene
  tmp = String(foo)
  tmp.length
  delete tmp
  ```
  Or
  ```jsx
  const foo = 'bar'
  new String(foo).length
  ```

So, the reason why {} or Object can be used exchangeable even if

```tsx
Object === {} // are not equal
```

is because an empty object ( {} ) is **instance of Object** or **technical instance of built-in constructor function( Object).**

```tsx
{} instanceof Object // is True
// If you wanna try it, place {} in variable
// just like this
const EmptyObj = {}
EmptyObj instanceof Object
```

<aside>
💡 We can also apply this in JavaScript

For Example:

```jsx
let value = new String()
let something
if (value instanceof Object) {
	something = 'It is instance of object'
	console.log(something)
}
// In this case the output is "It is instance of object"
```

But if we did this

```jsx
let value = null | undefined
let something
if (value instanceof Object) {
	something = 'It is instance of object'
	console.log(something)
}
// In this case the output is "undefined"
```

</aside>

For a Deep Explanation about `Object` and `{}` also about `object` 😕 check out this: [Link](https://yeabsiramekuria.vercel.app/blog/difference-between-object-object-and--in-typesript)
