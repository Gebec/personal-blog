---
title: "Function overloading in Typescript"
date: 2022-05-14
author: "Gebec"
techs: ["js"]
tags: ["Typescript"]
categories: ["Typescript", "Guide"]
draft: false
---

Function overloading is very common technique in many programming languages. JavaScript itself this feature do not support. But luckily, we have TypeScript!
Implementation of function overloading in TypeScript is different to some languages, because it uses only single implementation. The overloading is expressed with many overload signatures that the client will call to invoke the implementation signature.

### Example without function overloading
There might be a function that will greet a person every time it is called.
```js
function greet(personName: string): string {
    return `Hello, ${personName}!`;
}

greet('Adam');
```
This function accepts one argument, name of the person to greet, of type string.

But have to modify the function so it also accepts array of persons to greet. The modified function will accept single string or an array of strings as argument and will return string or an array of strings.

The greet function can be modified as follows:
```js
function greet(personName: string | string[]): string | string[] {
    if (typeof personName === 'string') {
        return `Hello, ${personName}!`;
    }

    const arr: string[] = [];
    for (const name of personName) {
        arr.push(`Hello, ${name}!`);
    }

    return arr;
}

console.log(greet('Adam')); // 'Hello Adam!';
console.log(greet(['Bob', 'Carl'])); // ['Hello Adam!', 'Hello Bob!']
```
This modification is good and valid, but there are situations where it is not enough. For example, if we try to save the output to a variable, we get a TypeScript Error:
```js
const greetAdam: string = greet('Adam');
// TS Error: Type 'string | string[]' is not assignable to type 'string'.
```
To fix this Error, we may use the function overloading.

### Function overloading
Function overloading is very useful when we have function that uses many types as arguments and/of return types. To create function overloading, we have to define implementation signature and overload signatures.

##### Implementation signature
It is the implementation of functionality that actually do any computation and returns values. The implementation signature cannot be called directly, but must be called through its overload signatures.
When we are rewriting piece of code to function overloading, the JavaScript function remains the same and becomes the implementation signature.

To change out greet function into implementation signature, we have to only change the types to be more general.

##### Overload signatures
Overload signatures are TypeScript function definitions, that are meant to describe every possible combination of arguments and return types our implementation signature can handle and can be invoked with. We can define as many overload signatures as we need.
Every overload signature, if called with proper arguments, will invoke implementation signature.

If our function is called with string parameter, it should return string (first overload signature).
If is called with array of strings, it should return array of strings (second overload signature).

The greet function rewritten to function overloading:
```js
// Overloading signatures
function greet(person: string): string;
function greet(persons: string[]): string[];

// Implementation signature
function greet(personName: unknown): unknown { // <-- Only this line got changed
    if (typeof personName === 'string') {
        return `Hello, ${personName}!`;
    }

    const arr: string[] = [];
    for (const name of personName) {
        arr.push(`Hello, ${name}!`);
    }

    return arr;
}
```
We can call the function as usual but now we can also save the function output to variables without any error.
```js
console.log(greet('Adam')); // 'Hello Adam!';
console.log(greet(['Adam', 'Bob'])); // ['Hello Adam!', 'Hello Bob!']

const greetAdam: string = greet('Adam');
const greetTeam: string[] = greet('Bob', 'Carl');
```

### When to use function overloading
The main use case where the function overloading should be used is when we have multiple functions with the same name but different in parameter and return types.

### When to avoid function overloading
For use cases when to avoid function overloading, please refer to the (TypeScript Do's and Don'ts)[https://www.typescriptlang.org/docs/handbook/declaration-files/do-s-and-don-ts.html#function-overloads].

### Conclusion
Function overloading allows to define functions that can be called in multiple ways.

To change our function into function overloading, we have to create overload signatures, that sets function types with parameter and return types, and an implementation signature, the function body.

Main advantages of function overloading is that the code is more reusable with increased readability and maintainability of the code.
