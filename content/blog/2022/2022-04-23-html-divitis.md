---
title: "HTML Divitis problem"
date: 2022-04-23
author: "Gebec"
techs: ["html"]
tags: ["HTML", "Short"]
categories: ["HTML"]
draft: false
---

***Divitis*** or ***div soup*** is quite common problem of frontend developers. It is a habit or a process of using too many unnecessary div HTML elements in place of other meaningful semantic HTML elements.

### Purpose of `<div>`
`<div>` element is a generic container without any effect on the content or layout, it should be used to group content to be easily styled with CSS or accessed with JavaScript. The element does not represent anything, but allows developers to turn it into almost everything. The purpose is added with CSS (styling), JavaScript (functionality) or ARIA (accessibility).

The use case of `<div>` element is best described on [MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/div#usage_notes):
>   The `<div>` element should be used only when no other semantic element (such as `<article>` or `<nav>`) is appropriate.

### Reasons for 'divitis'?
The overuse of `<div>` is most probably caused by developers lack in knowing semantic meaning of other semantic elements, such as `<main>`, `<article>` or `<header>`.

Reasons we use `<div>`:
- wrapping elements to use `class` or `id` attributes
- changing page structure to alter visual layouts
- to mark-up items that does not match any other HTML element

### Divitis example
Have you ever looked on the source code of your gmail?

It may looks something like this:
```html
<body ...>
    <div style="position: relative; min-height: 100%;">
        <div class="vI8oZc yL">
            <div class="nH" style="width: 1920px;">
                <div class="nH" style="position: relative;">
                    <div class="nH bkL">
                        <div class="no">
                            <div class="nH oy8Mbf nn aeN" style="width: 187px; height: 832px;">
                                <div class="Ls77Lb aZ6" style="">
                                    <div jscontroller="DUNnfe" class="pp" style="user-select: none;">
                                        <div id=":q1">
                                            <div class="nM">
                                                <div id=":ps" class="aic">
                                                    <div class="z0">
                                                        <div class="T-I T-I-KE L3" style="user-select: none" role="button" ...>Compose</div>
                                                        ...
```
I wonder if they are allowed to use any other HTML element other than `div`. Even the Compose button is a div...The gmail is made almost from only div elements.
The `<div>` and `<span>` elements are non-semantic elements, they do not say anything about the content they wrap. Just look on the gmail code example - can you tell how the page structure or layout looks like?

### How to avoid div overuse?
Just get familiar with all the [semantic HTML elements](https://developer.mozilla.org/en-US/docs/Glossary/Semantics#semantic_elements) available and start using them.
The usage of semantic elements makes our mark-up more meaningful and descriptive to developers, browsers and screen readers. When a `<form>` or `<table>` tag is used, everyone instantly knows, what is its content responsible for.

Them main semantic elements to use instead of div are:<br />
[&lt;nav&gt;](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/nav)<br />
[&lt;header&gt;](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/header)<br />
[&lt;main&gt;](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/main)<br />
[&lt;aside&gt;](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/aside)<br />
[&lt;article&gt;](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/article)<br />
[&lt;section&gt;](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/section)<br />
[&lt;footer&gt;](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/footer)
