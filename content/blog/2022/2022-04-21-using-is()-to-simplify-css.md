---
title: "Using :is() pseudo-class to simplify css selectors"
date: 2022-04-21
author: "Gebec"
techs: ["css"]
tags: ["CSS", "Tips", "Short"]
categories: ["CSS", "Tips"]
draft: false
---

The [`:is()` pseudo-class](https://developer.mozilla.org/en-US/docs/Web/CSS/:is) function in CSS allows to write shorter compounded selectors. It takes list of selectors as its argument and selects any element that can be selected by any of the selectors in that list.

Lets imagine you want to style `h1` and `p` in a `header` element:
```css
header h1,
header h2,
header h3,
header p {
    color: purple;
}
```

This is where the `:is()` pseudo-class come in handy. The rulesets can be shorten to just this nice one-liner:
```css
header :is(h1, h2, h3, p) {
    color: purple;
}
```

We can even get crazy and combine more `:is()` together:
```css
:is(header, main, footer) :is(h1, h2, h3, p) {
    color: purple;
}
```

That one-liner is similar to:
```css
header h1, header h2, header h3, heeder p,
main h1, main h2, main h3, main p,
footer h1, footer h2, footer h3, footer p {
    color purple;
}
```

Anyway, did you spot the typo in the last example? The typo has one big impact on the selector list - the color will not be applied to any element. The whole list is not valid because of that one typo...
But the `:is()` got you covered. It will ignore all invalid selectors but apply styles to all valid. The argument list of selectors passed to the pseudo-class is of type [forgiving-selector-list](https://drafts.csswg.org/selectors-4/#typedef-forgiving-selector-list). The `forgiving-selector-list` parses each selector individually and ignores any that fail to parse.

### Specificity
It seems that everything must have a downside. And `:is()` has one too - specificity.

The `:is()` is applying specificity of its most specific selector to all other arguments. That means that all the selectors have the same specificity that is equal to the most specific selector.
If we have something like `:is(#nav, .heading, p)`, the `#nav` has the highest specificity and that specificity will be applied to both remaining selectors. The `#nav` and the `p` will have the same `1 0 0` specificity.

### Browser support
The `:is` is supported by [all major browsers](https://caniuse.com/css-matches-pseudo) except IE. So if you don't have to support IE, it is absolutely save to use and we should be using the pseudo-class in our projects.
