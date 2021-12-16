---
title: "Meet lobotomized owl (selector)"
date: 2021-12-14
author: "Gebec"
techs: ["css"]
tags: ["CSS", "Short"]
categories: ["CSS"]
draft: false
---

A lobotomized owl is a eerie name for a pretty neat CSS selector. This selector was introduced in [2014 by Heydon Pickering](https://alistapart.com/article/axiomatic-css-and-lobotomized-owls/) to solve some flow content issues with just single line selector.

### Meet the Lobotomized Owl selector
```css
* + *
```

It may be an old concept but even with all those new CSS features it is still the best way how to control content spacing on your page. At least in my eyes.

The CSS seems quite simple, but what does it mean? We can read the selector as: select any element (second `*`) that is placed immediately after (`+`) any element (first `*`). In other words select all adjacent elements except the first one.

```html
<ul>
    <li></li> <!-- won't be selected -->
    <li></li> <!-- selected -->
    <li></li> <!-- selected -->
</ul>
```

This selector is very generic and it can target anything on the page. We have to limit the scope where the Owl can select element. We have to make it 'content aware'.

```css
.container > * + * {}
```

### Usage of the selector
The main usage for this selector is to control the flow on a page. To control horizontal or vertical spacing between elements on a page or container.
There exist some other ways but mostly you will end up with a problem how to change spacing of the first element.
The Lobotomized Owl will not select the first element that is easy win for us.

Check out the following CodePen to compare Lobotomized Owl selector with some other ways how to control vertical spacing.

{{< codepen id="OJxpyPQ" >}}

The worst alternative is to use negative margin on a parent. I had to add one extra div wrapper so that the negative margin can take effect.

And the best alternative is the `:not(:first-child)` selector. But be aware that the `:not()` selector is not yet fully supported, almost 92% according to [Can I use](https://caniuse.com/?search=%3Anot()).

### Usage in horizontal flow
The selector is in  most cases used in vertical flow control, but it can also be used for horizontally aligned elements. For example navigation menu or breadcrumbs, where the element are spaced in the same manner as in the vertical flow control.

### Summary
For me, the Lobotomized Owl selector is the best and most elegant way how to control horizontal or vertical spacing of elements. It is easy to use and remember, fully supported and infinitely reusable. Because all alternatives use the universal selector too, the performance should be similar. But anyway, performance of CSS selectors doesn't matter with todays browsers and computers.
