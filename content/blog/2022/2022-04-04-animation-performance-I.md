---
title: "CSS animation performance I"
date: 2022-04-04
author: "Gebec"
techs: ["css"]
tags: ["CSS", "Performance", "Animation"]
categories: ["CSS"]
draft: false
---

Have you ever wondered why some animations are slow and others not? If the animation is not performing well, it can negatively impact the user experience. And this should be nightmare of every frontend developer.

This multi part guide should explain the difference between different approaches to animations and ways how to measure performance.

### TL;DR
- Animating HTML elements requires system resources
- The more HTML elements is animated, the more system resources is required
- Always try to have at least 60 fps
- Different CSS properties require different amount of system resources
- Changing layout of the page (geometry of elements) is particularly expensive
- If you can, use only `transform` or `opacity`

### Painting events
Anytime when CSS properties are animated, a series of events is triggered. Those events are: Layout -> Paint -> Composite.

##### Layout event
It is a process of calculating how much space every element takes up and where is its position on screen. Every element affects others elements - width and height affects its position on the screen and size of its children elements.

Change of CSS properties that affects another elements (most of the properties - `width`, `height`, `margin`, `padding`, `border`,...) will trigger Layout event. These properties are changing geometry of a element and the browser have to check all other elements and "reflow" the page. Layout event will trigger Paint and Composite.

##### Paint event
It is the process of filling in pixels. It is drawing texts, colors, images, borders, shadows,... The paint is usually done on multiple layers.

CSS properties that trigger Paint event are for example: `color`, `background-color`, `border-color`, `text-decoration`, `z-index` and others. Changing any of these properties does not affect geometry of elements so Layout event can be skipped, but Paint event will still trigger Composite.

##### Composition
Because the parts of the page were probably drawn on multiple layers, they are in this stage drawn on the screen in correct order. This is important when one element overlaps another - the elements are drawn on the screen in correct order. This event requires **least system resources**.

CSS properties triggering only Composite are `transform` and `opacity`. As the geometry and all the pixels remain the same Composite event does not trigger any other event.

### Layout recalculation
Every change in elements dimension will affect all other elements because they have to recalculate its size and position on the screen. The screen will have to be than re-rendered. This counts only for element that are not positioned absolutely, because absolutely positioned elements change only its position (and all of its children) but does not affects other elements on the page - they are in its own layer. Absolutely positioned elements trigger all three events, but only on smaller portion of elements.

### Element transformation
Using CSS `transform` property does not change its geometry. It is only moving with the transformed element without affecting any other element on the page. Most of the browsers have implemented some computation and graphical optimizations without triggering repainting. Thats why `transform` (and `opacity`) is so performant compared to other CSS properties.

### Comparison of CSS and JS animations
- Composition is handled on thread known as the **"composition thread"** which is running on the GPU. This is different thread from the browser's "main thread". If the browser is running some expensive work on the main thread, the animation can still run without interruption. Using only CSS with the composition will make the CSS faster than JS animation.
- If any animation, CSS or JS, triggers Layout or Paint, the "main thread" will be required to do work. The Layout and Paint will most likely overhead any work with CSS or JS and the performance comparison will be probably same for both methods.
