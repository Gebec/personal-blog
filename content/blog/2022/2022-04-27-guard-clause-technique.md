---
title: "Avoid nesting if-else, use guard clauses technique instead"
date: 2022-04-27
author: "Gebec"
techs: ["js"]
tags: ["Javascript", "Short", "Refactoring"]
categories: ["Javascript", "Tip"]
draft: false
---

*Guard clauses* (guard, guard code or guard statement) is technique to write cleaner and more readable code.

### Guard
The definition of *guard clause* by [Wikipedia](https://en.wikipedia.org/wiki/Guard_(computer_science)):
>   Guard is a boolean expression that must evaluate to true if the program execution is to continue in the branch in question.

In other words if we have to use `if` statement, we should avoid using `else`. If the condition is evaluated to true, the program will perform all actions in `if` block and than exit the subroutine.

### Nested if-else example
```js
function hell() {
    if (isOnline) {
        if (isLoggedIn) {
            if (isAdmin) {
                doSomeAdminStuff();
            } else {
                showWarning('You are not allowed to perform this action');
            }
        } else {
            showWarning('You are not logged in. Please log in.');
        }
    } else {
        showWarning('You are offline!');
    }
}
```

Here are 3 nested if-else blocks. To perform desired function `doSomething()` we have to be online, logged in and have an admin role. Otherwise different messages are shown to the user.

Even though the conditions are pretty simple, the code is harder to read than it should be. It is difficult to track which message will be shown and it is prone to a typo. Instead the *guard clause* technique should be used.

### Example code refactoring
To refactor the code to be *guard* we have to reverse all the conditions. When the condition will be evaluated to true the code will perform the action and exit the function so the code execution will not continue with the next `if` condition.

```js
// if-else
if (isOnline) {
    // ...
} else {
    showWarning(...);
}

// guard clause
if (!isOnline) {
    showWarning(...);
    return;
}
```

The whole example function can be refactored to:
```js
function notHellAnymore() {
    if (!isOnline) {
        showWarning('You are offline!');
        return;
    }

    if (!isLoggedIn) {
        showWarning('You are not logged in. Please log in.');
        return;
    }

    if (!isAdmin) {
        showWarning('You are not allowed to perform this action');
        return;
    }

    doSomeAdminStuff();
}
```

There is no extensive indentation and the code is much more readable. The code can be very easily extended with another condition.

### Conclusion
Every programmer spend most if his time reading code. It is very important to use our time on reading but not trying to understand the logic. The guard technique is one of many techniques that will help us keep our code simple and readable.
