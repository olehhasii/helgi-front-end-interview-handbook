---
sidebar_position: 1
---

# General and fundamental concepts of JS

## JavaScript Basics and Internals

### 1. What is JavaScript?
JavaScript (JS) is a high-level, interpreted, dynamic, single-threaded programming language primarily used to create interactive behavior on web pages. 

### 2. What are ES5, ES6+, and ESNext?
ES5 and ES6+ are versions of ECMAScript, the standardized specification of JavaScript. ESNext refers to the latest features currently being added to the language.

### 3. What is a JS Engine?
A JavaScript engine is a program that executes JavaScript code — for example, **V8** (Chrome, Node.js), **SpiderMonkey** (Firefox), or **JavaScriptCore** (Safari). It parses, compiles, and optimizes JS for execution. JS is Interpreted or just-in-time compiled. It means that the entire code is converted into machine code at once, then executed immediately.

### 4. What is a JS Runtime?
A runtime provides the **environment** where JS code executes — it includes:
- The JS engine
- APIs (like DOM, `setTimeout`, or `fetch`)
- Event loop and callback queue  
- Call Stack: Keeps track of the execution context (function calls). Heap: A memory space used for storing objects. 

For example, Node.js runtime includes file system and network APIs, while browsers provide DOM and Web APIs.

### 5. What is the Call Stack and how does it work?
The **Call Stack** is a data structure that keeps track of function calls in JavaScript. It follows the **LIFO (Last In, First Out)** principle.

#### How it works:
1. When a function is called, it's **pushed** onto the stack
2. When a function completes, it's **popped** off the stack
3. The stack is **single-threaded** - only one function executes at a time
#### Example:
```javascript
function first() {
    console.log('First function');
    second();
    console.log('First function ends');
}

function second() {
    console.log('Second function');
    third();
    console.log('Second function ends');
}

function third() {
    console.log('Third function');
}

first();

// Output:
// First function
// Second function  
// Third function
// Second function ends
// First function ends
```
:::danger[What is Stack Overflow?]

**Stack Overflow** occurs when the stack exceeds its limit (usually ~10,000 calls)

:::

### 6. What is the Event Loop? How does it work?
The **Event Loop** is the core mechanism that allows JavaScript — a **single-threaded** language — to handle **asynchronous operations** without blocking the main thread. It handles asynchronous code by constantly checking the **call stack** and **task queues** (mircor/macro tasks), executing tasks when the stack is empty.
#### How the Event Loop Works:
1. **Call Stack** — place where JavaScript keeps track of the execution of functions. When a function is called, it’s added to the call stack, and when the function finishes execution, it is removed from the stack.
2. **Macrotask Queue** — place where asynchronous tasks (like setTimeout callbacks, I/O operations, or DOM events) wait to be executed.
3. **Microtask Queue** — microtask queue is for microtasks like `Promises`, `queueMicrotask()`, `process.nextTick()`, `MutationObserver` callbacks. Microtasks have higher priority, so JS processes all microtasks before any next macrotask.
4. **Event Loop** — continuously checks:
   - *Is the call stack empty?*
   - *If yes, take the next task from the microtask queue and push it to the stack.*
   - *Once the microtask queue is empty, it takes the first task from the macrotask queue and pushes it onto the call stack for execution.*

### 7. What are Microtasks and Macrotasks?
- **Microtasks** - are smaller async jobs (like `Promises`, `queueMicrotask()` mehod, `process.nextTick()`, `MutationObserver`)
- **Macrotasks** - `setTimeout`, `setInterval`, I/O operations

The event loop always empties the **microtask** queue before moving on to the next **macrotask**.

#### Example:

```javascript
console.log('1. Start');

// Macrotask
setTimeout(() => console.log('4. Macrotask'), 0);

// Microtask
Promise.resolve().then(() => console.log('3. Microtask'));

console.log('2. End');

// Output:
// 1. Start
// 2. End
// 3. Microtask
// 4. Macrotask
```

### 8. How does the event loop works with rendering (e.g., requestAnimationFrame)?
The browser event loop manages both JavaScript tasks and rendering. Rendering only happens between tasks, never in the middle of one. `requestAnimationFrame` schedules a callback to run right before the next repaint, making it ideal for smooth animations.

## Types, Values, Equality, Operators

### 1. What are JavaScript data types?
