---
title: "Notes on event delegation and triggering"
date: 2022-04-12
author: "Gebec"
techs: ["au"]
tags: ["Aurelia", "Short"]
categories: ["Aurelia"]
draft: false
---

Two main bindings to handle click events exist in Aurelia  - delegate and trigger. What are their key concepts and what is the difference between them? Let's take a closer look.

### Delegate
The delegate binding is recommended to be used as a default binding method to process most of the events. As every registered event listener requires allocation of memory, this binding uses the [event delegation technique](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Building_blocks/Events#event_delegation) to minimize the required system resources by registering only single listener.

According to the [documentation](https://aurelia.io/docs/binding/delegate-vs-trigger#delegate-vs-trigger), Aurelia registers single listener on a page's root element. It can be a `<body>`, Aurelia's root component or any other top level element. This listener is waiting for any click event to bubble up from elements in the application. When an event is registered, the Aurelia's event handler goes through all elements (starting from `event.target` up to the root) and checks if any delegate callback for given event type is on that element specified. If so, it calls the callback.

But not all the events can bubble. Event delegation cannot be used for events like `blur`, `focus`, `load` or `unload`, because they do not bubble thorough the DOM and the delegate bindings will not have any effects on them.

### Trigger
The trigger binding registers event listener on given element for every trigger binding used. That means that the same amount of used trigger bindings is the same as amount of registered listeners. And this will require much more memory to be allocated.

When we click on an element, it triggers the click listener registered on that element and calls the callback function. The listener also captures all click events from all children elements that bubbles up in the DOM tree.
Aurelia creates the listener on the element, all the rest is done by browser.

### Difference between delegate and trigger
The main problem and confusion about difference between delegate and triggers lies in a fact that Aurelia does not check the disable state of elements. It purely relies on browsers to do this checks.

This means that when we use delegate, Aurelia will only trigger delegate callbacks, but will not check the disable state on elements in the event path. If we have button with children elements, the disabled state on the button will be ignored.
```html
<button click.delegate="submit()" disabled.bind="isLoading">
    <svg-icon icon="letter"></svg-icon>
    <span>Submit</span>
</button>
```

If we click on the icon or on the text, the `submit()` will be called, but the disabled state will be ignored, there is no check in the [Aurelia code](https://github.com/aurelia/binding/blob/master/src/event-manager.js#L70), if the delegate callback is on the disabled element.

But with the trigger binding, the situation is different.
```html
<button click.trigger="submit()" disabled.bind="isLoading">
    <svg-icon icon="letter"></svg-icon>
    <span>Submit</span>
</button>
```

Aurelia creates event listener on the button element. When we click icon or text, the event will bubble to the button and the event listener will check the disabled state of the button. If it is disabled, the click event will be ignored and event listener will not trigger the callback function.

This is the reason, why delegate is not working in some cases and is recommended to use trigger instead.

### Summary
Use the delegate binding as much as possible. It will not make any big difference in speed, but makes a huge difference in memory usage. But be aware about the 'bug' in the precessing of delegated events - no checks if the elements are disabled in Aurelia code. This may creates tones of problems with submitting of wrongly filled forms.
When you are creating button with elements or adding elements to existing button, do not forget to use the trigger binding!
