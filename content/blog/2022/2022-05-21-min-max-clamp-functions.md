---
title: "CSS min(), max() and clamp()"
date: 2022-04-21
author: "Gebec"
techs: ["css"]
tags: ["CSS", "Tips", "Short"]
categories: ["CSS", "Tips"]
draft: false
---

To improve responsiveness of a site, we are used to to use CSS media queries, calc() function or even Web API ResizeObserver event listener.
But CSS has some function specifically designed to adjust element size within certain limits.

Let's have a look at them...

### What are min(), max() and clamp() functions?
These function belongs to [Math functions](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Functions#math_functions) category, together with `calc()` and `abs()`. The math function allows numeric values to be written as mathematical expression.

All functions can be used with any property which accepts a numeric value. That means any length, percentage or integer. To name some: `width`, `height`, `font-size`, `animation-duration`, `z-index`,...<br />
The mathematical expressions can use addition (+), subtraction (-), multiplication (*) or division (/) operators. For example `200px + 10vw`.

### Min function
The `min()` function takes two or more arguments and returns the smallest as a value for a CSS property.

```css
.max-width {
    width: 50vw;
    max-width: 500px;
}

.min-fn {
    width: min(50vw, 500px);
}
```
In the example above, the `min()` function will return 50vw unless it is bigger than 500px, than it will return 500px.
When we are on small screen sizes, the width will be 50%. But if we grow our screen enough to be wider than 1000px, the width will be 500px and not growing any bigger.

### Max function
The `max()` function is the opposite to `min()` function. It also takes two or more arguments, but it will return the biggest value of all of them.
```css
.min-width {
    width: 50vw;
    min-width: 200px
}

.max-fn {
    width: (50vw, 200px);
}
```
Here we have the similar example as in the `min()` function. The width will have minimal size of 200px and ideal 50vw. On screens and tables it will always be 50vh, but on mobiles with screen width 400px and less the element will have width of 200px as minimal width.

### Clamp function
The `clamp()` takes exactly three arguments. If provided less or more than three arguments, it will not work.
For the `min()` and `max()` functions, the order of values does not matter, it will always choose the smallest or highest value. But `clamp()` requires the values to be in specific order or it may not function as required.

This function combines the `min()` and the `max()`function. The first argument sets the minimal value that the function will return. On the opposite, the last value is the maximum value that will be returned. And the middle argument is so called ideal value. It is the value that will be returned when between the minimum and the maximum.


```css
.min-max-width {
    width: 50vw;
    min-width: 150px;
    max-width: 500px;
}

.clamp-fn {
    width: clamp(150px, 50vw, 500px);
}
```
Here in the example the width will have minimal width of 150px and maximum of 500px. Between the boundaries it will take 50% of the screen width.

##### When to use clamp()
**Fluid font-size**
Have you ever used a fluid font size? The `calc()` function is quite... hard to read. The calculation for [fluid font-size](https://css-tricks.com/snippets/css/fluid-typography/):
```css
body {
  font-size: calc([minimum size] + ([maximum size] - [minimum size]) * ((100vw - [minimum viewport width]) / ([maximum viewport width] - [minimum viewport width])));
}
```

But with `clamp()` it can be simplified to just this:
```css
body {
    font-size: clamp(16px, 4vw, 22px);`
}
```
The font-size will be 16px on mobiles and 22px on screens and fluidly changing between these values when changing screen width.

**Adaptive section padding**
The clamp() is ideal for setting minimal and maximal spacing between sections. Usually some media queries would be used, but with clamp(), it is as easy as
```css
section {
    padding: clamp(1rem, 5vw, 3rem) 1rem;
}
```

### Conclusion
All `min()`, `max()` and `clamp()` are very powerful CSS functions. The readability may be little bit harder at first, but once I got used to them, it got much easier to understand it.

The `min()` and `max()` are being used mostly with width and height.
And `clamp()` is so powerful with font-size that you will forget all other methods in a blink. Also if you use media queries to specify size on mobile, tablet and desktop, `clamp()` can solve it in one line with added dynamic resizing bonus.

All the function are supported in [all browsers except IE](https://caniuse.com/css-math-functions), so it is save to use them.
