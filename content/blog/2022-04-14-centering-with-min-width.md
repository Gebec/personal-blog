---
title: "Using modern CSS horizontal centering techniques"
date: 2021-12-14
author: "Gebec"
techs: ["css"]
tags: ["CSS", "Short"]
categories: ["CSS"]
draft: false
---

Centering element horizontally... Something you did many times in the past and for sure you will do many times as part of your daily job in the future.

CSS for such job can looks like this:
```css
.container {
    width: 100%;
    max-width: 62.5rem;
    padding: 0 1rem;
    margin: 0 auto;
}
```

Quite standard. `width` is used to set the element width to be as wide as possible but max to 62.5rem (the `max-width` property). `padding` for some spacing on left and right and `margin` to have the element centered.

But, what if we can use some of new css features and shorten that selector to just two lines with the same result?

### min()
[min()](https://developer.mozilla.org/en-US/docs/Web/CSS/min) is CSS function that takes list of parameters and uses the smallest one as a value for a given CSS property. `min()` can be used with any property that uses length, time, number, integer or angle as value. And `width` is one of them!
The function requires at least one parameter, more is optional.

```css
width: min(50vw, 200px);
```

In this example, the width will be 200px at most. But when the viewport is less than 400px wide, the `50vw` will be applied (50% from 400px is 200px) and the element will be wide 50% of the viewport.

How we can use this function in our centering problem? Like this:
```css
width: min(100% - 2rem);
```

We will have the element as wide as possible, but without `2rem`s. Why without `2rem`s? Because of the padding on the container (1rem on each side). Now we can ged rid of the padding. The `min()` function took care of it.
In the original container example I used `max-width` property. The `min()` can take care of this too! Remember, it will use the lowest value from all the specified parameters. So if we use both `(100% - 2rem)` and `62.5rem`, the function will select the percentage value, when the viewport is below 64.5rem and the rem value when over.

In the end we combined 3 properties into one!
```css
width: min(100% - 2rem, 62.5rem)
```

### margin-inline
[margin-inline](https://developer.mozilla.org/en-US/docs/Web/CSS/margin-inline) is a neat shorthand property that can be used too. `margin-inline` defines starting and ending value for margin depending on the element orientation. Namely on `writing-mode`, `direction` and `text-orientation` properties. But usually in most cases, like in 99.999%, the start will be left and the end will be right, or vice versa :)

We can use the `margin-inline` for the horizontal centering without setting the `margin-top` and `margin-bottom` values. So no more unnecessary overrides or no [margin collapsing](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Box_Model/Mastering_margin_collapsing) to take care of.
```css
margin-inline: auto
```

### Horizontally centered element with modern CSS
The result is shortened too:
```css
.container {
    width: min(100% - 2rem, 62.5rem);
    margin-inline: auto;
}
```

What about browser support? It is quite good: [min()](https://developer.mozilla.org/en-US/docs/Web/CSS/min) is over 90 % and [margin-inline](https://caniuse.com/mdn-css_properties_margin-inline) slightly bellow 90 %. For me, it is save to use.

One has to get used to using the `margin-inline` as it is not so obvious, but the `min()` is so cool! Don't you think?
