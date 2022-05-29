---
title: "How to use <section> HTML tag"
date: 2022-05-24
author: "Gebec"
techs: ["html"]
tags: ["HTML", "Short"]
categories: ["HTML"]
draft: false
---

Section tag is one of HTML semantic elements, that tell browsers how they should appear and what they do. What should section contain and how should be properly used?

### What is `<section>` used for?
A `<section>` is a semantic element for creating standalone sections in a document. The `<section>` element should only be used if there isn't a more specific element to represent the related content. For example, you wouldn't use a `<section>` element for a footer content — the footer element should be used instead. The `<section>` element should be used to divide up a one-page document into sections of related content.

{{< codepen id="JjppwPm" tab="result,html" >}}

### Why should be the `<section>` used?
The use of semantic elements will convey the meaning of the elements they contain. This helps to search engines, browsers, screen readers and of course other developers understand the different parts of the page.

### How to use the `<section>`?
To add a meaning to the `<section>`, we should follow some rules that make sure the `<section>` will be treated correctly by browsers and screen readers.

##### Section must have a header
The `<section>` element should almost always have a heading. This heading can be any of `<h1>` to `<h6>` elements. Also the heading can be nested in a `<header>` element. Because if `<header>` is nested in a `<article>`, `<aside>`, `<main>`, `<nav>` or `<section>` it does not create a banner landmark because in this case its aria role is set to `generic`, so the `<header>` is save to use in here.

##### Add exact semantic meaning to <section>
The [MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/section) states that the `<section>` will have implicit ARIA role set to region, if:
> the element has an accessible name, otherwise no corresponding role

This means, that we have to explicitly set a name to the `<section>`, otherwise it will not have any role and that means that it will not have any semantic meaning, and that means that it will be treated as a `<div>` tag, but with a longer tag name.

The name or meaning can by added with two attributes: `aria-label` or `aria-labelledby`.
The `aria-label` takes a string as its value and the `aria-labelledby` takes an `id` ref as its value.
I personally prefer the `aria-labelledby`, because we have to change the accessible name only on one place.

```html
<section aria-labelledby="contact-us">
    <header>
        <h3 id="contact-us">Contact us</h3>
        <p>Let us know what you thing about this feature</>
    </header>
    <form>
        ....
    </form>
</section>
```
In the example above, the section `aria-labelledby` is referencing the `id` of the `<h3>` tag. If we change the heading, we don't have to change the meaning of the section accessible name. It will stay the same. For example, if we change the 'Contact us' to 'Call us 24/7', we are all covered.

### Conclusion
The `<section>` can be used to divide the document into standalone sections of a related content. Using the `<section>` element over a generic element like `<div>` can help make your code more accessible and understandable to search engines, browsers, screen readers and other developers.
To have the `<section>` working as expected, always add a heading and accessible name to it.
