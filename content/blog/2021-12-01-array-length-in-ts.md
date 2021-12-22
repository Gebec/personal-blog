---
title: "How to define an array of minimal length in Typescript"
date: 2021-12-01
author: "Gebec"
techs: ["ts"]
tags: ["Typescript", "Tips", "Short"]
categories: ["Typescript", "Tips"]
draft: false
---

To define an array of minimal length we have to create an array of defined length and then extend it with an array of unknown length.


Array of certain length can be done in few ways
```js
// classical array
type ArrayOfOne<T> = [T];
type ArrayOfTwo<T> = [T, T];

// object-like list
type ArrayOfOne<T> = { 0: T };
type ArrayOfTwo<T> = { 0: T, 1: T};
```

And the extension can be done with intersection or with spread operator.
```js
// Intersection
type ArrayOfOneOrMore<T> = [T] & T[];
type ArrayOfOneOrMore2<T> = ArrayOfOne & T[];
type ArrayOfOneOrMore3<T> = { 0: T } & T[];

// Spread operator
type ArrayOfOneOrMore<T> = [T, ...T[]];
type ArrayOfOneOrMore2<T> = [ArrayOfOne, ...T[]];
type ArrayOfOneOrMore3<T> = [{ 0: T }, ...T[]];
```

We can now use the types to ensure that the array has the required minimal length.
```js
function example(items: ArrayOfOneOrMore<number>) {}

// Error: Source has 0 element(s) but target requires 1.
example([]);

// Valid
example([1]);
example([1, 2]);
example([1, 2, 3]);
```

I personally prefer using the spread operator which is easier for me to read and grasp. And also the typescript warning message is much easier to understand.
