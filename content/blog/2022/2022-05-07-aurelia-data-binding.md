---
title: "Data binding commands in Aurelia"
date: 2022-05-07
author: "Gebec"
techs: ["au"]
tags: ["Aurelia", "Short"]
categories: ["Aurelia",]
draft: false
---

Data binding is a fancy name for setting relation for data passed between a parent component and a child component. Aurelia recognizes 5 types of data binding: `one-time`, `one-way` (or `to-view`), `from-view`, `two-way` and `bind`.

### One-time
This data binding method is the most simplest one. It passes the data from parent to the custom component only once during the bind lifecycle method. As the value is not considered to change, Aurelia will not create a watcher for the value change.

```
<custom-component text.one-time="'This is custom component text'"></custom-component>
```

##### Notes on one-time data binding
If you forget to add the data binding command in your custom component call, the one-time binding will be used.
```
<custom-component text="'No command specified'"></custom-component>
```
The value of `text` will still be passed to the `custom-component`, but with one-time binding. Aurelia will not react on `text` value updates.

If you are using binding attributes with the same name as any of standard HTML attributes, never forget to use data binding command. Without the command the value will be passed to the custom component, but because it is a valid HTML attribute, it can have a side effect in the document.
```
<custom-component id="'component'"></custom-component>
```
Here you will end up with two valid 'component' ids on the page. One on the <custom-component> HTML tag and one inside the custom component's HTML.

### One-way
This data binding is the standard one to update value when any action is finished, like fetching data from server, form validation messages or setting values after some heavy calculations.

```
<validation-errors errors.one-way="validationErrors"></validation-errors>
```
The value of errors can be null, undefined or an empty array until user will fill form with a incorrect value. After that we can create a user-friendly error message(s) that will be shown to user to inform him that something is wrong.

##### Notes on one-way data binding
One-way data binding is not really a one way, there is a way how to update the value from the child component. If the value is a object, than passed is its reference, not the value itself. In the child component we can mutate the object's value and the change will be propagated to the parent component. So it is always good to set the value of one-way property as readonly in the child component if possible.

### From-view
From-view is used in forms where the input values are changed only by user but not by the JavaScript.
This is the least used binding command in my opinion. Even though the bind command is more resource heavy, it is always safer to use bind (or two-way) over from-view.

### Two-way
This data binding is for cases when both model and view can modify the value. Usually in forms, where user can fill in value into input or the input can change in response to a action (fetching saved data from a server, filling predefined options on user selection, etc.).

### Bind
The bind data binding is the no brain choice as it will automatically choose between one-time and two-way data binding.

```
<custom-component user-name.bind="userName"></custom-component>
```

##### Notes on bind data binding
In the end we are left with two commands to choose between - one-time and bind. Always choose bind if you do not need one-time :)
In case of bind, Aurelia will choose what is the best for given property. So it can react on changes in the code and we do not have to keep in mind to change the command whenever necessary.
