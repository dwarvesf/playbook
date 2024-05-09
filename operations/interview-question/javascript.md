---
tags: 
  - operations
  - hiring
  - frontend
  - javascript
title: "Frontend interview questions: Javascript"
date: 2020-01-01
description: The sample to check candidate quality in interview round
authors: 
  - hnh
menu: playbook
type: null
hide_frontmatter: false
---

## For all level
### Closure
<aside>
🎯 Check the understanding of closure in JavaScript. The answer would be a clear definition of closure and an explanation that all functions in JavaScript are closures, and maybe a few more words about technical details: the `[[Environment]]` property and how Lexical Environment work.

</aside>

Ask the candidate to complete function `a()`

```jsx
function a(){}

console.log(a(1)(2)(3));
// 6
console.log(a(1)(1)(1));
// 3
```

Once the candidate can give a similar below answer

```jsx
function a(x){
   return function b(y){
      return function c(z){
         return x + y + z;
      }
   }
}
```

Then ask him **what the characteristic of Javascript that could enable the code to work is**. If the answer is closure, ask him to explain in detail.

<aside>
ℹ️ It’s also good if the candidate can mention First-Class Function from the example where a function can return a function (as being treated as a value).

</aside>

### Event loop
<aside>
🎯 Evaluate the understanding of Browser JavaScript execution flow via event loop. Understanding how event loop works is important for optimizations, and sometimes for the right architecture.

</aside>

Hand the candidate one of the following snippet and ask him to explain **how browser engines handle the code**. 

```jsx
// setTimeout
setTimeout(() => { console.log(1) });
console.log(2)
```

```jsx
// Promise
const promise = new Promise((resolve) => resolve(1));
promise.then((res) => console.log(res));
console.log(2)
```

If the answer is `event loop`, ask the candidate for his understanding. Otherwise, we can consider skipping the question.

![](assets/eventloop.png)

If the candidate can give a good explanation and you consider he is an experience one, ask him to evaluate the code:

```jsx
setTimeout(()=> console.log(1));
const promise = new Promise((resolve)=> resolve(2));
promise.then((res) => {console.log(res)});
console.log(3);
```

In some browsers (Chrome, for example), it will print out

```jsx
// 3 2 1
```

Instead of

```jsx
// 3 1 2
```

Ask the candidate for the log output and why it will not print out `3 1 2`. This should check his understanding of `Microtasks` and `Macrotasks`.

### Event Bubbling and Capturing
**Questions to ask**: This handler is assigned to `<div>`, but also run if you click the nested `button`. Ask the candidate why does the handler on `<div>` run if the actual click was on `button`?

```jsx
<div onclick="alert('The handler!')">
  <button>Click me</button>
</div>
```
**Answer**
***Bubbling***
When an event happens on an element, it first runs the handlers on it, then on its parent, then all the way up on other ancestors.

```jsx
<form onclick="alert('form')">
  <div onclick="alert('div')">
    <button onclick="alert('button')">Click me</button>
  </div>
</form>
```

A click on the inner `<button>` first runs `onclick`:
1. On that `<button>`.
2. Then on the outer `<div>`.
3. Then on the outer `<form>`.
4. And so on upwards till the `document` object.

![](assets/eventbubbling.png)

<aside>
💬 The key word in this phrase is “almost”.
    
For instance, a `focus` event does not bubble. There are other examples too, we’ll meet them. But still it’s an exception, rather than a rule, most events do bubble.
    
</aside>

***event.target***
The most deeply nested element that caused the event is called a *target* element, accessible as **`event.target`**

Note the differences from `this` (=`event.currentTarget`):

- `event.target` – is the “target” element that initiated the event, it doesn’t change through the bubbling process.
- `this` – is the “current” element, the one that has a currently running handler on it.

***Stopping bubbling***
Any event handler can stop the event by calling `event.stopPropagation()`, but that’s not recommended, because we can’t really be sure we won’t need it above, maybe for completely different things.

For instance, here `body.onclick` doesn’t work if you click on `<button>`:

```jsx
<body onclick="alert(`the bubbling doesn't reach here`)">
  <button onclick="event.stopPropagation()">Click me</button>
</body>
```

- ℹ️  **event.stopImmediatePropagation()**
    
    <aside>
    💬 If an element has multiple event handlers on a single event, then even if one of them stops the bubbling, the other ones still execute.
    
    In other words, `event.stopPropagation()` stops the move upwards, but on the current element all other handlers will run.
    
    To stop the bubbling and prevent handlers on the current element from running, there’s a method `event.stopImmediatePropagation()`. After it no other handlers execute.
    
    </aside>
    
**Don’t stop bubbling without a need**
    
<aside>
💬 Bubbling is convenient. Don’t stop it without a real need: obvious and architecturally well thought out.
    
Sometimes `event.stopPropagation()` creates hidden pitfalls that later may become problems.
    
For instance:
    
1. We create a nested menu. Each submenu handles clicks on its elements and calls `stopPropagation` so that the outer menu won’t trigger.
2. Later we decide to catch clicks on the whole window, to track users’ behavior (where people click). Some analytic systems do that. Usually the code uses `document.addEventListener('click'…)` to catch all clicks.
3. Our analytic won’t work over the area where clicks are stopped by `stopPropagation`. Sadly, we’ve got a “dead zone”.
    
There’s usually no real need to prevent the bubbling. A task that seemingly requires that may be solved by other means. One of them is to use custom events, we’ll cover them later. Also we can write our data into the `event` object in one handler and read it in another one, so we can pass to handlers on parents information about the processing below.
    
</aside>
    

***capturing***
The capturing phase is used very rarely, usually we handle events on bubbling. And there’s a logic behind that.

In real world, when an accident happens, local authorities react first. They know best the area where it happened. Then higher-level authorities if needed. The same for event handlers. The code that set the handler on a particular element knows maximum details about the element and what it does. A handler on a particular `<td>` may be suited for that exactly `<td>`, it knows everything about it, so it should get the chance first. Then its immediate parent also knows about the context, but a little bit less, and so on till the very top element that handles general concepts and runs the last one.

![](assets/eventbubbling1.png)


### Debouncing and Throttling
**Question**: What is debouncing? When should you use debounce?

**Answers**: The Debounce technique allows us to “group” multiple sequential calls in a single one. It forces a function to wait a certain amount of time before running again. The function can be implemented to invoke on the leading edge or the trailing edge of the timeout based on different use cases:

- Only send an HTTP request when users stop typing to avoid extra work for the server.
- Wait until a user stops typing before validating its input.

**Question**: What is throttling? When should you use throttle?

**Answer**: By using **throttle**, we don’t allow a function to execute more than once during a certain amount of time.

The main difference between this and debouncing is that throttle guarantees the execution of the function regularly.

Use case: When a user is scrolling down your infinite-scrolling page, check how far from the bottom the user is. If the user is near the bottom, we should send an Ajax request for more content.

**Question**: What is the difference between debouncing and throttling?

**Answer**: Debounce and throttle are two similar (but different!) techniques to control how many times we allow a function to be executed over time. Mostly, it’s for the concern of improving browser performance.

### var, let & const
> 🎯 Check the understanding of `var`, `let`, and `const` with respect to their script, use, and hoisting.
>> The answer to this question is useful mostly for understanding old scripts. So we expect only an experienced candidate or someone who takes the job of migrating old production code to answer the question.



### What is "this" keyword

## For middel-senior level 
### Event Delegation
### Memory leak
### Function binding
### DOM manipulation
### IIFE and Revealing Module Pattern

## For senior level
### Prototype
### Single-thread language