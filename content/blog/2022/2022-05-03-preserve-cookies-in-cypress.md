---
title: "How to stay logged in between Cypress tests"
date: 2022-05-03
author: "Gebec"
techs: ["cypress"]
tags: ["Cypress", "Short"]
categories: ["Cypress"]
draft: false
---

If you have a private area on your web page you probably had to solve a problem, how to stay logged in during Cypress tests. Because of performance, Cypress deletes all cookies before every test. But it will cause that you get logged out before every test.

### Brutal force
The first idea which usually comes to mind is to log in at the start of every test. After we get logged out, we will log in back again.

```js
import { loginUser } from '../utils';

describe('log in', () => {
    const userName = 'admin';
    const password = 'secret';

    beforeEach(() => {
        loginUser(userName, password);
    });
});
```

This method is not ideal, we have to send a request to the backend and wait for response - cookies that will log us back in. This may not sound like a big deal, but imagine tha you can have like 50 suites with around 5 test in each... In the end, this can be a very expensive method.

### Preserve cookies in beforeEach()
The quick fix to this problem is to preserve the cookies and set them at the start of every test - in `beforeEach()` hook. We will log in only once, and before every test we say that certain cookies should be preserved and not deleted.

```js
import { loginUser } from '../utils';

describe('log in', () => {
    const userName = 'admin';
    const password = 'secret'

    before(() => {
        loginUser(userName, password);
    });

    beforeEach(() => {
        Cypress.Cookies.preserveOnce('cookieName')
    });
});
```

This is logical and most used way how to stay logged in. Once we have the cookies set up, we just prevent their deletion.

### Setting Cookies defaults
Another option, how to keep cookies, is by modifying global defaults to preserve a set of cookies which will always be present across tests.

```js
import { loginUser } from '../utils';

describe('log in', () => {
    const userName = 'admin';
    const password = 'secret';

    Cypress.Cookies.defaults({
        preserve: 'cookieName'
    });
    before(() => {
        loginUser(userName, password);
    })
});
```

The `preserve` accepts string for single cookie name or array for multiple names. We can also use RegExp or function as a value.<br /> This method is very cool, because we can set our defaults in our `cypress/support/index.js` file. As the `index.js` is loaded before any test file, the defaults will be among the first values that will be set. This will leave us with only the `before()` hook to be repeated in every test suite.

### Conclusion
There are 3 ways how to stay logged in. For me, the method with setting the Cookies defaults is the best, because it has less code that has to be repeated in every test.
