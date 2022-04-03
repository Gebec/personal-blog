---
title: "Desing patterns in JS: Singleton"
date: 2022-04-02
author: "Gebec"
techs: ["js"]
tags: ["Javascript", "Design Patterns"]
categories: ["Javascript", "Guide"]
draft: false
---

Singleton pattern is a creational design pattern that restricts the initialization of a class (or object in general) to single instance while providing a global access point to this instance.

### Use cases for singleton
Usually singleton is used when we have to have only one single instance throughout the application. This can be for example:
- database connection - whole program works with one connection and it would not be practical to create new connection every time we need to connect to the database
- storing global state shared across application - user language, application paths, settings,...
- logging
- caching


### Overview
- Singleton must have only one instance. The best way is to make sure, that the singleton itself is responsible for keeping track of its sole instance. Singleton can ensure that no other instance can be created and provides a way to access the instance.
- The singleton instance must be accessible to a client from a well-known access point.

### JavaScript implementation
##### Class singleton
To get always the same instance, the singleton must return its instance if already instantiated.
```js
if (Singleton.instance instanceof Singleton) {
 return Singleton.instance;
}
Singleton.instance = this;
```

This code should be placed in a constructor so it can return Singleton instance every time (second and any other time) the constructor is called.

*Full code example*
{{< codepen id="PoEORdg" >}}

##### Function singleton
Singleton can be also implemented in functional-based javascript. But it is not used as often as in class-based JavaScript. Usually the framework you use has its own way how to implement *'singleton'* (f.e. [useContext](https://reactjs.org/docs/context.html) hook in React).

The singleton as function must be an [IIFE](https://developer.mozilla.org/en-US/docs/Glossary/IIFE) that will return the same object all the time.
```js
// private property hidden in scope
let instance;
// publicly accessible logic
if (!instance) {
    instance = getInstance()
}
return instance;
```

*Full code example*
{{< codepen id="JjMOvdZ" >}}


### Pros of singletons
- We can be sure that the class has only one instance
- Singleton object is initialized only when is required for the first time

### Cons of singletons
- In most cases used unnecessarily
- Violates [Single responsibility principle](https://en.wikipedia.org/wiki/Single-responsibility_principle). The singleton is responsible for its normal tasks, but it must also ensure that only one instance is created.
- Can mask bad design, f.e. when the components know too much about each other
- Can be hard to unit test the code that uses singletons when creating mocked objects.
