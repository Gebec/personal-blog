---
title: "How to create enums in JS or TS"
date: 2022-06-07
author: "Gebec"
techs: ["js"]
tags: ["Javascript", "Typescript" "Short"]
categories: ["Javascript"]
draft: false
---

JavaScript does not offer any build in way how to create enums so we have to use a workaround. On the other hand, TypeScript has its way, or two, how we can create enums.

### What are enums
Enums are used in situations where only limited amount of predefined constants is allowed. For example, the traffic lights can have only 3 colors - red, yellow and green.

```js
let trafficLight = 'red';

// after some time, the color will change
trafficLight = 'yellow';

// and the color will eventually become green so we can finally go
trafficLight = 'green';
```

### Enum in JavaScript
To define an enum-like object in JavaScript we can crete something like this:
```js
const TRAFFIC_COLOR = {
    RED: 'red',
    YELLOW: 'yellow',
    GREEN: 'green'
}
```

The uppercase is used because of convention, but there is no need to follow this convention and you can write it in camelCase.

So out previous code can be rewritten to use the enum-like object:
```js
let trafficLight = TRAFFIC_COLOR.RED;

// after some time, the color will change
trafficLight = TRAFFIC_COLOR.YELLOW;

// and the color will eventually become green so we can finally go
trafficLight = TRAFFIC_COLOR.GREEN;
```

The problem with this enum-like object is that it is ordinary object and as such it can be modified at any time:
```js
TRAFFIC_COLOR.RED = 'green';

// or
TRAFFIC_COLOR.BLUE = 'blue';
```

To prevent this behavior and mimic the real enum, we have to freeze the enum-like object so it cannot be changed.
```js
const TRAFFIC_COLOR = Object.freeze({
    RED: 'red',
    YELLOW: 'yellow',
    GREEN: 'green'
})

// if we try to modify the enum object
TRAFFIC_COLOR.RED = 'green';
TRAFFIC_COLOR.BLUE = 'blue';

// nothing gets changed
console.log(TRAFFIC_COLOR);
// prints: { "RED": "red", "YELLOW": "yellow", "GREEN": "green"}
```

In the previous example we successfully created in plain JS an object that has all the properties of enum used in other languages.


### Enum in TypeScript
TypeScript has [enums](https://www.typescriptlang.org/docs/handbook/enums.html) as build in feature.

So we can simply write an enum in a `.ts` file as:
```ts
enum TRAFFIC_COLOR = {
    RED: 'red',
    YELLOW: 'yellow',
    GREEN: 'green'
};

// or even without values if we do not need them
enum TRAFFIC_COLOR = {
    RED,
    YELLOW,
    GREEN
};
```

### const enum alternative in TypeScript
If one option is not enough, TypeScript offers another way, how to create enums. It differs only that it is defined with the `const` keyword.

```ts
const enum TRAFFIC_COLOR = {
    RED: 'red',
    YELLOW: 'yellow',
    GREEN: 'green'
};
```

There are some subtle differences in the generated JavaScript code. For example the bundled size for the `const` variant is quite lower, because the transpiler will use the enums values directly and not the enum object itself.
But this approach may come with a cost. For more info please refer to the [TS documentation](https://www.typescriptlang.org/docs/handbook/enums.html#const-enums).

### Conclusion
Enums are powerful objects, that stores some limited amount of unique constant values. They are common in many languages, but unfortunately, JavaScript does not have them build in. Luckily in JavaScript we can mimic the behavior of enums by creating a freezed object.
TypeScript even offers two ways how to create enums, every having its drawbacks.
