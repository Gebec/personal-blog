---
title: "Event capturing and bubbling in Javascript"
date: 2021-12-22
author: "Gebec"
techs: ["js"]
tags: ["Javascript", "Guide"]
categories: ["Javascript", "Guide"]
draft: false
---

Every event is a special object used in browsers to signal that something happened to a HTML element. And javascript can be used to react to that fired object. Event can be fired on almost any thinkable action - pressing a key, clicking a button, tapping on a phone or signaling that the page is loaded. In addition to plenty of predefined events we can even create our own custom events and dispatch them at any time we want.

Javascript uses two processes to handle the event - event listener and event handler. Event listener is something that is sitting on a page (or more exactly on a HTML element) and waiting for given event to happen. Event handler is block of code that will be run as a response to the event. When an event is registered by event listener, the listener will call the event handler to perform action we defined.

The event listener can be defined in two ways:
```html
<button onclick="doSomething()"></button>
```
In this first case, the onclick attribute is an event listener and doSomething() is an event handler defined as an action that should be called when we click on the button.

```html
<button></button>
```

```js
const button = document.querySelector('button');

button.addEventListener('click', doSomething());
```
Here we define the event listener in our js file. The 'click' is the event name we are listening for and the doSomething() is an event handler used as a callback in the addEventListener method. The event listener can have [third optional parameter](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener#parameters). It can be an object of additional options or a boolean value. The boolean value sets the capturing flag.


### Event Propagation
Now when we know what is an event and how to listen and react to one, we can now dive into some practical stuff.

Let's imagine we have two div element wrapping around a button and every element has an onclick event listener registered that will show an alert window. You can try it:

{{< codepen id="WNZZLVP" height="350">}}

<br/>

Clicking on the button will show 3 alerts in this order: button, parent 2 and parent 1.

To understand what happened, we have to think about the event itself. At first, when we click on a button, the browser creates DOM event. In case of a click it is a [PointerEvent](https://developer.mozilla.org/en-US/docs/Web/API/PointerEvent).

The PointerEvent carries a lot of information (see the console), to pick some:
```js
{
    bubbles: true,
    path: [button.click-listener, div.wrapper-2.click-listener, div.wrapper-1.click-listener, body, html, document, Window],
    pointerType: "mouse",
    screenX: 612,
    screenY: 360,
    target: button.click-listener,
    type: "click"
    ...
}
```

<br/>

Note the type property that is set to 'click', stating that the event represents a mouse 'click'.

After creating the event object, the browser dispatches it. The event goes throw the DOM in a process called Event Propagation. And there are three phases in the event propagation process:

#### 1. Capturing phase
In the capturing phase, the event goes from root of the DOM traversing every element until it reaches the target element that triggered the event. In our example of PointerEvent (see the json example), it goes `html -> body -> div.wrapper-1 -> div.wrapper-2 -> button`. On every element the event is traversing, it checks if it has registered event listener of its type and with `capture` flag set to true. The default value of the capture parameter in the event listener is set to false. So the event listeners does not listen to events in the capturing phase.

#### 2. Targeting phase
The event reaches the target element and runs an event listener attache to it if set. That gives us the first alert window.

#### 3. Bubbling phase
In this phase the event traverse from event target back up to the DOM's root. It is similar to the capturing phase but in reverse direction.
All the listeners with 'capture' flag set to false (all the defaults) are called. Here we got the remaining alert windows.

The Event propagation in our case can be drawn as below.
{{< image src="images/posts/event-propagation.png" caption="Event Propagation phases" alt="Image of Event propagation phases" position="center" class="mx-auto d-block" >}}
