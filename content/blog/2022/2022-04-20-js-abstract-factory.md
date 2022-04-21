---
title: "Design patterns in JS: Abstract factory"
date: 2022-04-20
author: "Gebec"
techs: ["js"]
tags: ["Javascript", "Design Patterns"]
categories: ["Javascript", "Guide"]
draft: false
---

Abstract factory is a creational design pattern that creates objects that follow a general pattern. Abstract factory is considered to be an another layer of abstraction over factory pattern. An abstract factory abstracted out a theme that is shared by the newly created objects.

### Introduction
> Provide an interface for creating families of related or dependent objects without specifying their concrete classes.
> * GOF

Usually the abstract factory is used to create an implementation of the abstract factory and then uses the generic factory interface to create concrete objects.
Abstract factory uses inheritance and relies on subclasses to handle the implementation. All the subclasses will share the same theme - methods defined in the abstract factory, but implemented in subclasses.

### Implementation in JS
Unfortunately JavaScript does not support interfaces, so abstract factory pattern cannot be implemented in a nice and elegant way.
But we can use a object's prototype to implement another object as an interface...

Our abstract factory:
```js
function AbstractFactory() {
    this.createProduct = function() {
        throw new Error('createProduct must be defined in subclasses')
    }
}
```

Every concrete factory will have `createProduct()` method inherited from `AbstractFactor()`. But the implementation of this abstract method is done in the concrete factory (or subclass):
```js
ConcreteFactory.prototype = new AbstractFactory();

function ConcreteFactory() {
    this.createProduct = function() {
        return 'New product created';
    }
}
```

And for example we can create another factory with different `createProduct()` implementation:
```js
AnotherFactory.prototype = new AbstractFactory();

function AnotherFactory() {
    this.createProduct = function() {
        return 'Brand new polished product created';
    }
}
```

Our object created from abstract factory will looks like this:
```js
const factory = new ConcreteFactory()
factory.createProduct()
// Output: 'New product created'
```

### Implementation in TS
In TypeScript the implementation is much easier and better to understand.

We again start with creating abstract factory with methods or properties. But in this case as a interface:
```ts
interface IAbstractFactory {
    createProduct(): string;
}
```

To use the abstract factory interface we use TypeScript`s `implements` keyword:
```ts
class ConcreteFactory implements IAbstractFactory {
    public createProduct(): string {
        return 'Created product in TypeScript';
    }
}
```

And final object instantiation
```ts
const factory = new ConcreteFactory();
factory.createProduct();
// Output: 'Created product in TypeScript'
```

### Usage of abstract factory
The abstract factory is quite common in TypeScript code. It solves similar problems as factory method design pattern but with greater abstraction in the types of objects that can be created. It encapsulates the basic and shared methods for creating families or related objects. In other words it abstracts common behavior among components.
