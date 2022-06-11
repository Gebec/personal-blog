---
title: "Cypress bundled libraries"
date: 2022-05-28
author: "Gebec"
techs: ["cypress"]
tags: ["Cypress", "Overview"]
categories: ["Cypress"]
draft: false
---

Cypress relies on many open-source libraries. Mainly they are used internally, but they are also exported and we can use them in out tests. They may solve a lot of problems with ease so it good to know about them.

### Mocha
Cypress adopted [Mocha's BDD syntax](http://mochajs.org/), which is used for integration and unit testing. Every Cypress test sits on the fundamental harness Mocha provides. To be more exact, Cypress is extending Mocha and polishing some weird edge cases, bugs and error messages.

To name some Mocha's commands used in Cypress tests:
- `describe()`
- `before()`
- `beforeEach()`
- `it()`
- `.only()`
- `.skip()`

As you probably wrote plenty of Cypress tests, you are very familiar with these commands.

### Chai
[Chai](http://chaijs.com/) is providing the ability to do assertions in our tests. Its syntax makes the assertions easy to read and understand. The assertions are mostly used as `expect()` or `should()`

##### should()
The `should()` command is the number one command for assertion testing. It is used most of the time. It is chained to a cy element(s).

```js
cy.get('input[type=radio]').should('have.length', 3);

cy.get('#main').should('be.visible');

cy.get('.table').find('td').first().should('have.text', 'first column');
```

##### expect()
The `expect()` command is not chained and is standing on its own.

```js
expect(true).to.be.true;

const obj = { foo: 'bar' };
expect(obj).to.equal(obj);
expect(obj).to.deep.equal(obj);
```

### Chai-jQuery
When working with DOM, Cypress provides a [Chai-jQuery](https://github.com/chaijs/chai-jquery) library, that extends Chai with specific jQuery chainer methods.
The chainers are available when asserting about DOM objects with commands like `cy.get()` or `cy.contains()`.

```js

cy.get('input[type=radio]').should($inputs => {
    expect($inputs).to.have.lengthOf(3);
})

cy.get('#main').then($el => {
    expect($el).to.be.visible
})
```

### Sinon.JS
[Sinon.JS](http://sinonjs.org/) is used in unit tests to stub or spy methods. In Cypress, two methods are available (`cy.stub()`, `cy.spy()`), that return Sinon stubs or spies.

Sinon is first library in this list, that can be directly called on Cypress global object anywhere in the test with `Cypress.sinon`.

##### cy.stub()
The `stub()` method replaces function and records its calls. It is used in unit or component test to prevent the original function or method to be called. Instead original function/method can be substituted with a fake one serving only to verify, that the function/method was called.

##### cy.spy()
Cypress can 'spy' on methods. It will wrap the method, but not modify. The spy will record method invocation together with all arguments, but the original method will still be called.

```js
// Replace obj.method() with stubbed function
cy.stub(obj, 'method')

// Stubbing prompts and alerts
cy.window().then(() => {
    cy.stub(win, 'prompt').returns('prompt text');
    cy.stub(win, 'alert').as('alert');
});

// Replace a function with stub
let listenersAdded = false

cy.stub(obj, 'addListeners', () => {
  listenersAdded = true
});

// Spying a method calls
const obj = {
  foo() {},
}
const spy = cy.spy(obj, 'foo').as('foo')

obj.foo('foo', 'bar')
expect(spy).to.be.called
```

### Sinon-chai
The [Sinon-chai](https://github.com/cypress-io/sinon-chai) library provides a set of assertions for using the Sinon's `spy()` and `stub()` with the Chai library - it allows to make assertions on those methods.

```js
mySpy.should.have.been.calledWith("foo");
// or
expect(mySpy).to.have.been.calledWith("foo");
```

### Lodash
Cypress includes [Lodash](https://lodash.com/) and expose it as a `Cypress._`. Lodash makes JavaScript easier by taking the hassle out of working with arrays, numbers, objects, strings, etc.


```js
// get all items in a list and confirm that all values are unique
cy.get('li')
    .should('have.length', 4)
    .then(($el) => {
        Cypress._.map($el, 'innerText)
    }).then((values) => {
        const uniques = Cypress._.uniq(values);
        expect(uniques).to.have.length(4);
    })
```

### jQuery
Cypress contains also [jQuery](http://jquery.com/) that is exposed as `Cypress.$`. Under the hood Cypress uses the jQuery library very extensively. For example Cypress commands yields jQuery elements so we can call methods on them. Most of the time we are not using the `Cypress.$` object, but instead we are working with the jQuery element returned from selection command.

This library is used mostly to select elements or test single element's properties.
The most commonly used jQuery commands and selectors are for example: `:disabled`, `:visible`, `.text()`, `.val()`, `.attr()` and others.

##### :disabled selector
The [:disabled selector](https://api.jquery.com/disabled-selector/) is used to select disabled elements.
```js
// uses the build in jQuery selector
cy.get('button:disabled').should('have.length', 0);

// or confirm, that button is disabled
cy.get('[data-cy=disabled-button]').then(($btn) => {
    $btn.is(':disabled');
});
```

##### :visible selector
[:visible](https://api.jquery.com/visible-selector/) is another useful jQuery selector used to filter elements, that are visible on the page. But be aware that under the hood Cypress is overriding the visibility definition used in jQuery and uses its [own](https://docs.cypress.io/guides/core-concepts/interacting-with-elements#Visibility).
```js
// Preferred way to filter visible elements
cy.get('button:visible').should('have.length', 1);

// Selecting elements in two commands with limited retry ability
cy.get('button').filter(':visible').should('have.length', 1);

// Another way, how to confirm visibility
cy.get('[data-cy=visible-element]').then(($btn) => {
    $btn.is(':visible');
});
```

##### .val()
[.val()](https://api.jquery.com/val/) method returns the value of the selected element
```js
cy.get('input').should(($input) => {
  const val = $input.val()

  expect(val).to.match(/foo/)
  expect(val).to.include('foo')
  expect(val).not.to.include('bar')
})
```


### Conclusion
Most of the libraries that Cypress offers, are used on daily basis as they serve as the backbone for every test (Mocha, Chai,...). But if we look for example on the Lodash or jQuery, they usage is limited to edge cases. They will be used in situations when working with other libraries will be pain or unsolvable problem. But when Lodash or jQuery will be used to solve the edge case, they will shine and solve it with ease.
