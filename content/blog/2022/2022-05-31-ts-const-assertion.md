---
title: "Typescript const assertion "
date: 2022-05-31
author: "Gebec"
techs: ["ts"]
tags: ["Typescript"]
categories: ["Typescript", "Guide"]
draft: false
---

The *'const'* assertion was added to TypeScript in version 3.4. It sets all properties of an object to be readonly and arrays becomes readonly tuples. Typescript ensures, that the type of the object or array cannot be widened.

### Readonly objects
If `const` assertion is cast on an object, it will mark all properties as read-only and we won't be able to modify any of them.

Let's take `user` object with properties `id` and `username`:
```ts
// type {id: number; username: string; }
const user = {
    id: 123,
    username: 'Mike'
}
```

Its type are inferred as expected. But if we use the `const` assertion, the object types are marked as read-only.
```ts
// type: { readonly id: number; readonly username: string; }
const user = {
    id: 123,
    username: 'Mike'
} as const;
```

If we than try to update the username field, we would get this TypeScript error: `Cannot assign to 'username' because it is a read-only property`
```ts
user.username = 'John';
```

### Readonly arrays
If we use `const` assertion with an array it will make the array to be a read-only tuple. I.e. every value of the array becomes a literal type that cannot be modified.

We can have an array of numbers:
```ts
// type: number[]
const arr = [1, 2, 3, 4];
```

But if we use `const`, the type of the array becomes restricted:
```ts
// type: readonly [1, 2, 3, 4]
const arr = [1, 2, 3, 4] as const;
```

And again, if we try to modify the array, we get an error:
```ts
// Error: Cannot assign to '0' because it is a read-only property.
arr[0] = 10;

// Error: Property 'push' does not exist on type readonly [1, 2, 3, 4]
arr.push(5);
```

##### It just cannot be changed
When a `const` assertion is used, TypeScript can be sure that its contents will be the same at any time.

```ts
function sum([a, b]: [a: number, b: number]): number {
  return a + b;
}

const arr = [1, 2] as const;
console.log(sum(arr)); // Prints: 3
```

But without the assertion, this will throw an error.
```ts
function sum([a, b]: [a: number, b: number]): number {
    return a + b;
}

const arr = [1, 2];
// Error: Argument of type 'number[]' is not assignable to parameter
// of type '[a: number, b: number]'
console.log(sum(arr));
```

We can even try to remove the destructuring in the sum function, but still, the TypeScript will not be very happy.
```ts
function sum(a: number, b: number) {
  return a + b;
}

const arr = [1, 2];
// Error: A spread argument must either have a tuple type
// or be passed to a rest parameter.
console.log(sum(...arr));
```
In both error examples, the problem is caused in the fact, that TypeScript cannot be sure, that the `arr` value is not changed between its declaration and `sum()` function call.


### Read-only, but...
The `const` assertion makes read-only only values but not variables. If we reference a variable as property value, it still can be changed.
```ts
const arr = [1, 2, 3, 4];
const foo = {
  name: "foo",
  contents: arr,
} as const;

foo.name = "bar"; // TS error
foo.contents = []; // TS error
foo.contents.push(5); // ...works!
```

### Conclusion
`const` assertion can be used to mark an object values as read-only, create a read-only Tuple, and mark a variable's value as a Literal type instead of widening it to its based type i.e. string, number, etc.
