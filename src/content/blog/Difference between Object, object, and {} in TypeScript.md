---
external: false
title: 'Difference between Object, object, and {} in TypeScript'
description: 'There is an entire difference and similarity between `Object`, `object`, and `{}` in TypeScript/JavaScript.'
date: 2023-03-22
---

#

There is an entire difference and similarity between `Object`, `object`, and `{}` in TypeScript/JavaScript.

## Object

`Object` is a built-in constructor function in JavaScript. In TypeScript, `Object` is a type that represents all non-primitive types, i.e., anything that is not `number`, `string`, `boolean`, `symbol`, `null`, or `undefined` , But it doesn’t include the object version (Constructor Object) of `number`, `string`, `boolean` or `symbol` which is

```tsx
let StringObj = new String()
let NumberObj = new Number()
let SymbolObj = new Symbol()
// so on ....
```

This means that you can use `Object` to represent any object, including `arrays`, `functions`, and `objects` created with `other constructor functions.`

```tsx
let obj: Object = {}
obj = new String() // valid
obj = new Array() // valid
obj = new Date() // valid

// We can also assign it to any primitive value except null and undefined

obj = 43243243
obj = true
obj = 'possible'
obj = ['sjdfhd', 'fsdafdsf']
obj = {
	value: 'anything',
}
```

## **object**

`object` is a type that represents any non-primitive type, similar to `Object`. However, it is different from `Object` in that it works `structurally`. let me clarify what do I mean by `structurally`.

```tsx
// If I declare variable with object type like this
let value: object
// the only thing I can assign to this is variable is something like this
value = {
	something: 'helloooooo',
}
// but if I assign it with primitive value
value = 'fsdfd' // Invalid
value = 34234 // Invalid

// anything that doesn't look like object is not valid for object type in typescript
// by the way object type is typescript specific
```

This means that you can also use `object` to represent any object except `null` and `undefined`.

```tsx
let obj: object = {}
obj = new String() // valid
obj = new Array() // valid
obj = new Date() // valid
obj = null // invalid, null is not an object
obj = undefined // invalid, undefined in not object
```

## **{}**

`{}` is an empty object literal, which is of type `Record<string, any>` in TypeScript. It can be used to create an object with no properties.

```tsx
let obj: {} = {}
obj = { name: 'John', age: 30 } // valid
obj = 344332 // valid
obj = 'fsdfd'
obj = null // invalid, null is not an object
obj = undefined // invalid, undefined in not object
```

Don't use the `Object` or `object` or `{}` type when typing objects, only use these type when you want to exclude nullish or undefined values (Object or {} ) or if you want any object value (object). The reason is you cannot access the method of the object if you typed the object using one of the three.

```tsx
let notPossible: Object = {
    value:'fljdksfja'
}
notPossible: {}= {
    value:'fljdksfja'
}
notPossible: object= {
    value:'fljdksfja'
}
notPossible.value // all of them will raise error => Property 'value' does not exist on type 'Object'
```

As a conclusion all of these(Object,object and {}) are useful when we want to accept or pass any data type but not null or undefined.
