---
title: "How to include custom components in Aurelia"
date: 2021-12-04
author: "Gebec"
techs: ["au"]
tags: ["Aurelia", "Guide"]
categories: ["Aurelia", "Tips"]
draft: false
---

Every framework has its ways how to include custom components, reusable block of codes, into the page content.

In Aurelia, we have three main options for this. Custom element tag, `as-element` attribute and compose tag. Every variant has it own use case, pros and cons. So let's dive into it.

### Custom element tag
The simplest and wildly used approach to include component is by using custom element tag.

```html
<template>
    <require from="./hello-world"></require>

    <hello-world></hello-world>
</template>
```

#### Use case
Nothing strange or special, just require the file with your component and include it with dash-cased class name tag. You will see being it used in documentations, every guide and Stack Overflow.

#### Pros
Simple, no special syntax and fastest to load and render.

#### Cons
The main disadvantage of this approach is the fact that you are using unknown html tag. This means that every browser will use its default user agent stylesheet to style the tag. For frontend developer this is a no way. Firstly, every browser has different default user agent stylesheets, so you can't be sure if the element will looks as expected and the same in every browser (even on all those mobile browsers). Secondly, the element will be styled as `display: inline`, which is... just wrong. You want inline for your `span`s, but for sure not for a wrapper element.

To overcome this issue, you can for example add class to the custom element tag that will set at least `display: block`.

### as-elements
Using `as-element` attribute is another way how to fix the styling problem with the custom element tag. It will use html tag of your choice (`div` tag most of the time) instead of the custom element.

```html
<template>
    <require from="./hello-world"></require>

    <div as-element="hello-world"></div>
</template>
```

#### Use case
When you do not want to deal with custom element tag issues.

#### Pros
Same or almost the same rendering speed as for custom component tag. More html friendly approach, you will see the power when you will be inspecting html and see the all the elements highlighted. It will make your CSS debugging so much easier.

#### Cons
You still have to use an attribute to force it to behave as a standard element.

### Compose
This one is special, it has its single use case where others can't be used. But for a big speed cost...

```html
<template>
    <compose view-model="getTemplate()"></compose>
    <div as-element="compose" view-model="getTemplate()"></div>
</template>
```

#### Use case
This custom element will be used when you don't know in front which element is going to be rendered. You can use one template when a certain condition is true and another template if condition fails. Or the content of an element is being build dynamically. For example you want to render a table where you have multiple column types and each has it own template - path to the template will be known much later in the component lifecycle (`bind()` or `attached()`).

#### Pros
Used for narrow use case where other types cannot be used. You don't have to write endless conditional binding like `if.bind="column.type == 'action'"`.

#### Cons
Well, it is slow as hell (like 4-5 times slower compared to custom component tag and `as-element`). So if any other approach can be used than the dynamic composition should be avoided.

### Containerless
This one is not a real way how to include custom element into page content, it is rather a hot fix how to "hide" unknown html tags. `containerless` is an element attribute that will remove the html tag of your custom element from the DOM.

```html
<!-- ./hello-world.html -->
<template>
    <span>Hello world</span>
</template>
```

```html
<template>
    <require from="./hello-world.html"></require>

    <div class="wrapper">
        <hello-world containerless></hello-world>
    </div>
</template>
```

Will be rendered as
```html
<div class="wrapper">
    <span>Hello world</span>
</div>
```

#### Use case
It can be used in cases where you want to remove the custom component tag and do not want to use an extra wrapper tag used with `as-element`.

#### Pros
Same pros as for `as-element`.

#### Cons
It is some javascript magic with heavy DOM manipulation that will cause very slow rendering...
