---
title: "Defer script attribute"
date: 2022-04-31
author: "Gebec"
techs: ["html"]
tags: ["HTML", "Short"]
categories: ["HTML"]
draft: false
---

The `defer` attribute is used with `<script>` where it signals to browsers that the external script should be downloaded in parallel to parsing the page and executed after the document is parsed but before [DOMContentLoaded](https://developer.mozilla.org/en-US/docs/Web/API/Document/DOMContentLoaded_event) event is fired.

### Example use case
Let's imagine that we have a button with a `onClick` event listener that will perform any action when we click on the button.

The HTML can look like this:
```html
<!doctype html>
<html>
  <head>
    <title>Defer attribute usage</title>
    <script src="src/script.js"></script>
  </head>

  <body>
    <button type="button" id="btn">Click me</button>
  </body>
</html>
```

And the script in 'src/script.js' is as simple as this:
```js
const btn = document.getElementById('btn');

btn.addEventListener('click', () => {
    alert('Button has been clicked!');
});
```

Can you guess what is the problem here? You can see the [plunker example](https://plnkr.co/edit/oU7xWd09NkP3djNF?preview).

The page will load and the button will render.

But the button does not react on our clicks.

When we inspect the console, we will see a ***TypeError*** :
`Uncaught TypeError: Cannot read properties of null (reading 'addEventListener') at script.js:3:5`

Did you get the problem now? The script will be loaded and executed **before** the page is loaded. So in the time of script execution, there is no element with `id="btn"` and the `getElementById()` will return `null`. And if we try to append event listener on `null`, JS will throw a *TypeError*...

### Workaround
There is one possible workaround how to deal with this type of errors - move the `<script>` tag bellow the button.
```html
<!doctype html>
<html>
  <head>
    <title>Defer attribute usage</title>
  </head>

  <body>
    <button type="button" id="btn">Click me</button>
    <script src="src/script.js"></script> <!-- <-- script moved here-->
  </body>
</html>
```

This is absolutely valid solution, because the script will be downloaded and executed after the `<button>` was parsed, so the `id="btn"` exists and there will be no *TypeError*.

But this is still a workaround, because for these cases `defer` attribute was added.

### The defer attribute
The HTML code is the same as in the first example with one exception - in the `<script>` tag is added `defer` attribute:
```html
<!doctype html>
<html>
  <head>
    <title>Defer attribute usage</title>
    <script src="src/script.js" defer></script>
  </head>

  <body>
    <button type="button" id="btn">Click me</button>
  </body>
</html>
```

If you look on a [plunker example](https://plnkr.co/edit/hXDIQSpMB2zZy6lZ?preview), no *TypeError* in the console and the button works as expected.

What exactly the `defer` attribute does? It tells the browser to divide it into two steps - downloading the script and executing the script. <br />
- The first step, the download, will be done in the background as soon as the browser gets to the `script` tag.
- The second step, the execution, will be done when the document is fully parsed, all the HTML code is ready, but just before the `DOMContentLoaded` event. The `defer` will block this event until all defer `<scripts>` are executed.

### `defer` of workaround?
The `defer` attribute should be preferred over the workaround. There are two reasons why.
1) The browser will start the script download when it gets to the line where the `<script>` is defined. In the defer method it will be very soon because the script is in the `<head>`. But in the workaround it may be anytime in the parsing process. Maybe just before the closing `<body>`. This may have an impact on the performance, depending on the script size and how long it takes to parse the document.
2) The `<script>` makes much more sense in the page head than in the body from semantic point of view. The `<head>` should contains all the external resources and the `<body>` should have the markup and the content.

### Conclusion
Use the `defer` attribute whenever the script interacts with the DOM. It will start downloading as soon as possible but will postpone the script execution until the document is parsed so it will prevent any errors caused by early script execution.
