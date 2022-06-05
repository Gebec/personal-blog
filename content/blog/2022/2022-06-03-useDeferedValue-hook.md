---
title: "React 18 useDeferredValue hook"
date: 2022-06-03
author: "Gebec"
techs: ["js"]
tags: ["React", "Guide"]
categories: ["React"]
draft: false
---

The React 18 introduces *Concurrent React*. It is React, that cares about tasks and their priorities to be processed by a browser in certain order. For this purpose, the React 18 introduced two new hooks: ***useTransition*** and ***useDeferredValue***.

This post is about the `useDeferredValue` hook

### The problem to solve
Usually, when we update the state, we expect the page to be re-rendered without any delay. But there might be situation, when we require to postpone the page re-rendering. Let's imagine, that the user can write something to an input and the page content changes as the user types. In this most simplest case, we always expect that the typed character immediately appears in the input.

```js
export const App = () => {
    const [value, setValue] = useState("");

    const handleChange = (event) => {
        setValue(event.target.value);
    }

    return (
        <React.fragment>
            <input type="text" value={value} onChange={handleChange} />
            <Results value={value} />
        </React.fragment>
    )
}
```
The `App` component is very simple component with single input element and a `<Results />` component with single prop passed down.

Let's have a look on the `<Results>` component. To simulate some heavy calculations, the `<Results />` component can look like this:
```js
export const Results = ({ value }) => {
    const count = 25000;

    let results = React.useMemo(() => {
        const list = []
        for (let i = 0; i < count; i++) {
            list.push(<li key={i}>{ value }</li>);
        }
        return list;
    }, [value]);

    return (
        <ul>
            {results}
        </ul>
    )
}
```

<br /><br />
Try type something in the input in the working example and see how the content is rendered and how the value in the input is updated.

{{< codepen id="MWQBQrQ" >}}
<br />
If you are typing, you can see, how the input text stays the same and is not updated on every key press. The browser is too busy with re-rendering, that it does not have time to update the text in the input. The text gets updated after the `<Results />` component is fully rendered.

### Fixing the performance issue
The problem is in the `<Results />` component. It is re-rendered every time, the input value changes. To fix this, we can implement the `useDeferredValue` hook to postpone the component rendering.

Let's defer the value:
```js
export const Results = ({ value }) => {
  const count = 25000;
  const deferredValue = React.useDeferredValue(value); // <-- here the value is deferred

  const results = React.useMemo(() => {
    const list = [];
    for (let i = 0; i < count; i++) {
      list.push(<li key={i}>{ deferredValue }</li>);
    }
    return list;
  }, [deferredValue]); // <-- now, we can wait until the deferred value gets updated


  return (
      <ul>
        {results}
      </ul>
  )
}
```
{{< codepen id="VwQBQPO" >}}
<br />
If you type fast enough, you can see, that the input value gets updated before the `<Results />` starts re-rendering.

### New way how to defer value
The `useDeferredValue` hook is similar to throttling or debouncing hooks to defer values. The difference between `useDeferredValue` and debouncing or throttling is in the way how they work.

When creating custom debouncing or throttling hooks, we have to specify exact time for how long the hook will defer its execution. On the other hand, the `useDeferredValue` works directly with JavaScript event loop, allowing all other tasks to be processed with higher priority. Once the que of the event loop is empty, the `useDeferredValue` hook will execute (low priority task).

We can say, that both throttling and debouncing are static hooks - they will update the value after the specified time has passed no matter how fast our computer is or if the calculations are finished or not. But the `useDeferredValue` will execute once the main thread does not have anything to do. It can be 1 ms or 1000 ms. It all depends on how resource heavy tasks the event loop has to process.

In the last codepen example. The typing has to be fast, because we have to keep the event loop busy. If the typing is slow, the event loop gets bored and this will trigger the `useDeferredValue` hook execution and component re-rendering.

### Memoizing the component
The `useDeferredValue` only defers the value passed as an argument, but not the component itself. If we want to prevent the component from re-rendering during urgent updates too (as we usually want), we have to memoize the component with `React.memo` or `React.useMemo`. By memoizing the component, we tell to React that we want to re-render the component only when the deferred value is changed.

### Conclusion
The `useDeferredValue` brings now tool into the React hooks toolkit. There is no need to specify fixed times for our custom throttling and debouncing hook. It will defer the execution of slow and blocking code until the event loop que is empty. This will make the application feels more performant and smooth.
