---
title: "Handling errors in try/catch"
date: 2022-06-21
author: "Gebec"
techs: ["ts"]
tags: ["Typescript", "Short"]
categories: ["Typescript"]
draft: false
---

In version 4.0 TypeScript fixed a 'bug' in a try/catch block, where the error was of type `any`. The `any` type allowed, well, anything... causing a lot of other problems when accessing properties that the error may not have without TypeScript to notice. Since version 4.0, the type was changed to `unknown` forcing developers to thing about the error itself end preventing blindlessly accessing properties without checks.

In this post I will look on the problem itself and show some ways how to determine thr proper type of the caught error.


### Caught error type problem
Because there are many ways how we can throw an error in JavaScript...
```ts
throw new Error('Error occurred!');
throw 404;
throw 'Another error';
```
...there are also many types the caught error can have.

Till TypeScript 4.0 the error in try/catch block was of type `any`. This allowed to access the `message` property on the error object, but it was considered as bad practice, because it was possible that the error is not an object and/or not having a `message` property...
```ts
// Till TS version 4.0
try {
  throw 404;
} catch (e) {
  console.log(e.message);
// Prints: undefined
}
```

But with version 4.0, the type of the error in catch clause got changed to `unknown`. Now TypeScript will complain, if we try to access the `message` property.
```ts
// Since TS version 4.0x
try {
   throw 404
} catch (e) {
  console.log(e.message);
  // TS error: Object is of type 'unknown'.
}
```

The exact reason why the default type was changed to `unknown` can be found in the [TypeScript docs](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-4-0.html#unknown-on-catch-clause-bindings):
> `unknown` is safer than `any` because it reminds us that we need to perform some sorts of type-checks before operating on our values.`

We can't specify the type in the catch clause. The TypeScript will not allow it:
```ts
try {
  throw 404;
} catch (e: Error) {
  // TS error: Catch clause variable type annotation must be 'any' or 'unknown' if specified.
}
```

So this new feature leave us with one new problem - we have to find out what type of error we got!

### Simple property check
With simple property check, we do not really check for the error type, but rather just checking, if the property is present on the object.

```ts
try {
  throw 404;
} catch (e: Error) {
  if (e.hasOwnProperty('message')) {
    console.log(e.message);
  }
}
```
If the `message` property is on the error object, the `hasOwnProperty` will evaluate to `true` and TypeScript will allow to access the property without any warning or alert.

I would call it the brutal force option. We don't get any additional information about the error. We are just checking, that the `message` property is present.

### Usage of type predicates
A [type predicates](https://www.typescriptlang.org/docs/handbook/2/narrowing.html#using-type-predicates) gives us a opportunity to specific type in a function. The type predicates uses type assertion in conjunction if/else block. If the check in a function is evaluated to true we will assert the type of a variable.
```ts
isNumber = (val: unknown): val is number {
  return typeof val === 'number';
}

try {
  throw 404;
} catch (e) {
  if (isNumber(e)) {
    // e is of type number
    return;
  }

  // e is of type unknown
}
```

The type predicates works best if we know in advance what types value can have. Here in the try/catch we are limited with a fact that the set of types of our error is almost infinite.
```ts
isNumber = (val: unknown): val is number {
  typeof val === 'number';
}

const stringOrNumber: string | number = getStringOrNumber();

if (isNumber(stringOrNumber)) {
  // stringOrNumber is a number
} else {
  // stringOrNumber is a string
}
```

### Usage of type guards
To narrow the types, we can use type guards. We can think of type guard to be an expression that performs a check that will guarantee the type in a given scope.

The main ways how to use type guards:
- with `in` keyword (or with `hasOwnProperty()`)
- with `typeof`
- with `instanceof`

##### Checking for specific property
This method of determining between types is not very useful for determining the type of the error. The reason is the same as with the type predicates - it is best suited for type unions.

If we have two object, each having its own unique properties, we can determine the type by checking if the object has the property.
```ts
try {
  throw 404;
} catch (e) {
  if (e.hasOwnProperty('message')) {
    const error: Error = e;
    console.log(error.message);
  }
}
```

##### Checking the typeof
`typeof` is quite known technique to determine the type. It works best with primitive types like number and strings.
```ts
try {
  throw 404;
} catch (e) {
  if (typeof e === 'number') {
    displayErrorPage();
    return;
  }
}
```

Or
```ts
try {
  throw 'Site not found';
} catch (e) {
  if (typeof e === 'string') {
    console.error(e);
    return;
  }
}
```

The typeof is not very useful for more complex structures like objects. TypeScript will not get any additional information if we say, that the value is a object. What properties does the object has? TypeScript will not know.

##### Checking the instance
Probably the most useful and clean technique to determine the type is the `instanceof` checking. With this keyword we can say that the value is of given type and assert that type in given block scope.

```ts
try {
  await getData();
} catch (e) {
  if (e instanceof Error) {
    // type of e is Error
    console.error(e.message);
    return;
  }
}
```
The `instanceof` will shine when we will throw ours custom Errors instead of a generic `throw new Error('message')`. In this case we can write very complex logic to handle any possible option.
```ts
try {
  await getData();
} catch (e) {
  if (e instanceof HttpResponseException) {
    // ...
  } else if (e instanceof InternalServerError) {
    // ...
  } else if (e instanceof SomeOtherError) {
    // ...
  } else {
    // fallback clause to handle all other types
  }
}
```

### Conclusion
Since the caught error can have any possible type, it's our duty to correctly determine what is its type to prevent any other problems. There are plenty ways how to check what the type is. It can be simple checking for property to determine custom error type.What technique to use highly depends on actions that will be performed with the error. If we need just to access the `message` property, we can simply check if the error has the `message`. But if we need to do anything more, more sophisticated methods has to be used.
