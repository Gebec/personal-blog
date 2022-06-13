---
title: "CSS currentValue keyword"
date: 2022-06-11
author: "Gebec"
techs: ["css"]
tags: ["CSS", "Short"]
categories: ["CSS"]
draft: false
---

The currentColor keyword refers to the text color on a element where the keyword is used. And it can be used on any property that accepts a color value. Border, box-shadow, background-color and others.

### What is `currentColor` good for?
For the simplest example, we can use it to have a `border-color` with the same color as the text it wraps.
```css
button {
  color: blue;
  border-color: currentColor;
}
```

Well, in the example above, the `border-color` property is not necessary, as the default value of `border-color` is `currentColor`. But in most cases, the border is not styled by every property, but with a shorthand, where the color has to be set.

{{< codepen id="QWQzNxW" >}}

In the codepen example, you can see that the border color is set with `currentColor` and the border has the same color as the text.

### More complex example
When working with buttons, it is a good practice to use `currentColor`, because you have to specify the color only on one place and all the rest will 'inherit' the color.
Than, if you want to restyle the button or do some animation (e.g.on hover), it is as easy as changing single value. Or maybe you are using some contextual classes to color the element, e.g. `.text-danger` or `.text-success`, the current color can be used to style the shadows, borders or `::before` and `::after` pseudo elements without need to define the same color on all properties.

Because I started with button, I will continue with button and let's make it more colorful!

##### The base
Let's define the base color and structure:
```css
button {
  color: hsl(180, 100%, 50%);
  border: .25rem solid currentColor;
  background: transparent;
}
```

This are the same properties as used in the showcase example above. The hsl specifies the aqua color. I'm using hsl because it is easier to work and play with. It may be harder to grab the color from the hsl wheel at first, but the more used, the easier is to read the value.

##### Shadows
I will add 3 shadows to the button. One for the text to reduce the contrast and two for the border.

```css
button {
  ...
  box-shadow: 0 0 1.5rem 0 currentColor inset, 0 0 2rem 0 currentColor;
  text-shadow: 0 0 0.5rem currentColor;
}
```

##### Pseudo element
To demonstrate the power of the `currentColor` I will also add a pseudo element to our button. Let's add some gloving on the ground.
```css
button::before {
  content: "";
  background-color: currentColor;
  ...
}
```

##### And here comes the magic
And now, if I want to change the color on hover, I can do it with only one single change - the `color` change.
```css
button:hover,
button:focus {
  color: hsl(46, 99%, 44%);
}
```
And all the colors on whole element will change accordingly!

See the final button and its on hover color change in the next codepen:

{{< codepen id="vYdvKvE" >}}

### Conclusion
The keyword `currentColor` is in CSS for more than decade, but I feel that the amount of developers using this 'first CSS variable' seems to be dropping. It can be used in many use cases and seems to be more intuitive in custom components than CSS Custom properties, since we have to define only the `color` property to set the `currentColor` value.

But the `currentColor` has its limitations as it can only inherit color from the element it is used on. So we cannot use `currentColor` with `background-color` property, because if the background has the same color as the text.. it will be one big colored rectangle.

But I see the `currentColor` to be very useful in some situations, where it allows me to write more cleaner non-repetitive code.
