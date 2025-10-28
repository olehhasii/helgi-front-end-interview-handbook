---
sidebar_position: 4
---

# Functions, Execution Contexts, Closures and this keyword

A function in JavaScript is a reusable block of code that performs a task or returns a value. Functions help organize logic, avoid repetition, and improve readability. Functions can be defined using declarations, expressions, or arrow functions—each with different behavior in hoisting, this, and syntax.

### What are function declarations, expressions, and arrow functions?
JavaScript offers three main ways to create functions, each with different behaviors and use cases.

#### Function Declaration:
- Hoisted: Can be called before it's defined
- Has its own `this`, `arguments`, and `super`

```javascript
function greet(name) {
    return `Hello, ${name}!`;
}

// Hoisted - can be called before declaration
console.log(greet("John")); // Works fine
```

#### Function Expression:
- Not hoisted (cannot be used before it's defined)
- Useful for dynamic or conditional definitions
```javascript
const greet = function(name) {
    return `Hello, ${name}!`;
};

// Not hoisted - must be declared before use
console.log(greet("John")); // ReferenceError if called before
```

#### Arrow Function:
- Not hoisted
- No own `this` → inherits from surrounding scope
- No `arguments`, `super`, or `new`
- Great for short callbacks or functional programming
```javascript
const greet = (name) => `Hello, ${name}!`;

// Concise syntax for simple functions
const add = (a, b) => a + b;
const square = x => x * x;
```

#### Key Differences:

| Feature | Declaration | Expression | Arrow |
|---------|-------------|------------|-------|
| **Hoisting** | ✅ Full hoisting | ❌ Not hoisted | ❌ Not hoisted |
| **`this` binding** | Own `this` | Own `this` | Lexical `this` |
| **`arguments`** | ✅ Available | ✅ Available | ❌ Not available |
| **Constructor** | ✅ Can use `new` | ✅ Can use `new` | ❌ Cannot use `new` |
| **Syntax** | `function name() {}` | `const name = function() {}` | `const name = () => {}` |

#### When to Use Each:

**Function Declaration:**
- When you need hoisting
- For main functions in your code
- When you need `arguments` object

**Function Expression:**
- For conditional function creation
- When assigning to variables
- For callbacks and event handlers

**Arrow Function:**
- For short, simple functions
- When you want lexical `this`
- In array methods (`map`, `filter`, `reduce`)
- For callbacks where `this` context matters

### How does this work in different function contexts?
The `this` keyword in JavaScript refers to the object that is currently executing the function. Its value depends on **how** the function is called, not where it's defined.

#### Global scope (non-strict): 
`this` refers to the global object (window in browsers, global in Node).
```javascript
function sayHello() {
    console.log(this); // Window (browser) or global (Node.js)
}

sayHello(); // Called without any context
```

#### Global scope (strict mode): 
`this` is undefined.

#### Object method: 
`this` is the object before the dot.
```javascript
const person = {
    name: "John",
    greet: function() {
        console.log(`Hello, ${this.name}`);
    }
};

person.greet(); // "Hello, John" - this = person object
```
#### Arrow function: 
`this` is lexically bound — it inherits `this` from its outer scope, not from how it’s called.
```javascript
const obj = {
    name: "John",
    regularFunction: function() {
        console.log(this.name); // "John" - this = obj
    },
    arrowFunction: () => {
        console.log(this.name); // undefined - this = global
    }
};

obj.regularFunction(); // "John"
obj.arrowFunction();   // undefined
```

#### Constructor (with new): 
`this` is the newly created object.
```javascript
function Person(name) {
    this.name = name;
    this.greet = function() {
        console.log(`Hello, ${this.name}`);
    };
}

const john = new Person("John");
john.greet(); // "Hello, John" - this = john instance
```

#### Event Handlers:
`this` usually refers to the element receiving the event (unless using arrow functions).
```javascript
const button = document.getElementById('btn');
button.addEventListener('click', function() {
    console.log(this); // button element
});

// Arrow function in event handler
button.addEventListener('click', () => {
    console.log(this); // global object (not button)
});
```
### What’s the difference between call(), apply(), and bind()?
`call()`, `apply()`, and `bind()` are methods that set `this` value when invoking or creating a function.

- `call(thisArg, ...args)`: Calls the function immediately with `thisArg` and arguments listed one by one.
```javascript
function greet(greeting) {
  console.log(`${greeting}, ${this.name}`);
}
const user = { name: "Jonh" };
greet.call(user, "Hello"); // "Hello, Jonh"
```
- `apply(thisArg, [args])`: Same as `call()`, but takes arguments as an array.
```javascript
function greet(greeting) {
  console.log(`${greeting}, ${this.name}`);
}
const user = { name: "Jonh" };
greet.apply(user, ["Hi"]); // "Hi, Jonh"
```
- `bind(thisArg, ...args)`: Returns a new function with `this` permanently bound. Doesn't invoke immediately.
```javascript
function greet(greeting) {
  console.log(`${greeting}, ${this.name}`);
}
const user = { name: "Jonh" };
const sayHi = greet.bind(user, "Hey");
sayHi(); // "Hey, Jonh"
```

### What is the difference between arguments and parameters?
- **Parameters** are the variables listed in a function definition.
- **Arguments** are the actual values passed when calling the function.

### What is the `arguments` object?
The `arguments` object is an array-like object available inside non-arrow functions that contains all arguments passed to the function. Accessible via `arguments[i]` and has a `length` property. It’s not a real array → no `map`, `forEach`, etc., unless borrowed.

### What is the function `.length` property?
The `.length` property of a function shows the number of parameters it is declared with, not how many were passed at runtime.

### What are execution contexts?
An execution context is the environment in which JavaScript code runs. It defines what variables, functions, and `this` are available.
- **Global execution context**: created when the program starts; sets up global object and scope.
- **Function execution context**: created whenever a function is called.
- **Eval context**: rarely used, created for `eval()`.

### What is functional programming?
Functional Programming (FP) is a coding style that treats computation as the evaluation of pure functions and avoids shared state and side effects. JavaScript supports FP because functions are first-class citizens—you can pass, return, and compose them like data.

### What does it mean that functions are first-class citizens?
In JavaScript, functions are first-class citizens, meaning they can be assigned to variables, passed as arguments, and returned from other functions. JavaScript treats functions like any other value. This allows for flexible, functional programming patterns. Enables callbacks, higher-order functions, closures. Core to event handling, array methods (map, filter, etc.)

### What are closures? What are their use cases?
A **closure** is a function that has access to variables in its outer (enclosing) scope even after the outer function has returned. Closures "remember" their lexical environment. Formed automatically when an inner function accesses variables from an outer function. The outer scope’s variables stay alive as long as the inner function is referenced. **Lexical scoping** - functions remember where they were defined

```javascript
function outerFunction(x) {
    // Outer function's variable
    return function innerFunction(y) {
        // Inner function has access to x
        return x + y;
    };
}

const addFive = outerFunction(5);
console.log(addFive(3)); // 8 - inner function remembers x = 5
```
#### Common Use Cases:
**1. Data privacy / encapsulation (simulate private variables).**
**2. Function factories (customized functions).**
```javascript
function createMultiplier(factor) {
    return function(number) {
        return number * factor;
    };
}

const double = createMultiplier(2);
const triple = createMultiplier(3);

console.log(double(5)); // 10
console.log(triple(5)); // 15
```
3. **Callbacks and event handlers that need access to surrounding state.**
```javascript
function setupButton(id) {
    const button = document.getElementById(id);
    let clickCount = 0;
    
    button.addEventListener('click', function() {
        clickCount++;
        console.log(`Button clicked ${clickCount} times`);
    });
}
```
4. **Memoization and caching of results.**

### What are pure functions and side effects?
A **pure function** always returns the same output for the same input and has no side effects. Pure functions are the building blocks of functional programming—they isolate logic and avoid surprises
A **side effect** is any observable change outside the function (e.g. modifying variables, DOM, API calls). Side Effects Include:
- Changing global variables or object properties
- DOM manipulation (element.innerText = ...)
- Network requests (fetch)
- Logging (console.log)
- Writing to localStorage, cookies, files

### What is mutability vs immutability?
 - **Mutable** values can be changed after creation
- **Immutable** values cannot be changed—a new copy must be created instead
Mutability affects how data flows. Mutable data can introduce hidden side effects, while immutability encourages clarity and control.

### What is an IIFE? Why use it?
IIFE (Immediately Invoked Function Expression) is a function that runs immediately after it’s defined, often used to create a private scope and act as a lightweight namespace. Wrapping code inside an IIFE prevents variables from leaking into the global scope.
```javascript
(function () {
  // private code here
  const secret = "hidden";
  console.log("IIFE ran");
})();
```
Use Cases: Avoid polluting global namespace, Create isolated modules, Encapsulate logic (before let/const and ES6 modules existed)

### What is a callback function?
A callback function is a function that is passed as an argument to another function, and called later, often after some task is complete. A callback is just a function used as data—often to delay or customize behavior

### What are higher-order functions?
A higher-order function is a function that takes another function as an argument, returns a function, or both. They enable code that is: Reusable, Modular, Declarative (less manual control, more "what to do").
Use Cases:
- Event handling (element.addEventListener)
- Functional composition
- Currying / partial application
- React: `useCallback`, `useEffect`, `setState(fn => ...)`

### What are first-class vs first-order vs unary functions?
Functions in JavaScript are first-class citizens—they can be: Assigned to variables, Passed as arguments, Returned from other functions
- A first-order function is a regular function that does NOT take or return another function.
- A unary function takes exactly one argument.

### What is debounce?
**Debounce** is a technique that limits the rate at which a function can fire. It ensures that a function is only called after a certain amount of time has passed since it was last invoked.
#### How Debounce Works:
- Function is called → timer starts
- If function is called again before timer expires → timer resets
- Function only executes when timer completes

#### Basic Implementation:
```javascript
function debounce(func, delay) {
    let timeoutId;
    
    return function(...args) {
        clearTimeout(timeoutId);
        timeoutId = setTimeout(() => func.apply(this, args), delay);
    };
}
```
#### Use Case Example:
- Without debounce: API is called on every keystroke
- With debounce: API is called only after the user stops typing for a short delay

### What is throttle?
**Throttle** is a technique that limits how often a function can be called. It ensures a function is executed at most once per specified time period, regardless of how many times it's invoked. Useful for events that should run at controlled intervals (scroll, window resize, mousemove).

#### How Throttle Works:
- Function is called → executes immediately
- Subsequent calls are ignored until time period expires
- After time period → function can be called again

#### Basic Implementation:
```javascript
function throttle(func, delay) {
    let lastCall = 0;
    
    return function(...args) {
        const now = Date.now();
        if (now - lastCall >= delay) {
            lastCall = now;
            func.apply(this, args);
        }
    };
}
```

### What is memoization?
**Memoization** is an optimization technique that stores the results of expensive function calls and returns the cached result when the same inputs occur again. It trades memory for speed.

#### Basic Concept:
- Function called with input → check cache
- If result exists → return cached result
- If not → compute, store in cache, return result

#### Simple Implementation:
```javascript
function memoize(func) {
    const cache = {};
    
    return function(...args) {
        const key = JSON.stringify(args);
        
        if (cache[key]) {
            console.log('Cache hit!');
            return cache[key];
        }
        
        console.log('Computing...');
        const result = func.apply(this, args);
        cache[key] = result;
        return result;
    };
}
```

### What is recursion?
Recursion is when a function calls itself to solve a smaller version of the original problem — until it reaches a base case that stops the calls. Base case – the condition where the function stops calling itself. Recursive case – where the function calls itself with smaller input
#### Basic Structure:
```javascript
function recursiveFunction(parameter) {
    // Base case - stops the recursion
    if (baseCase) {
        return baseValue;
    }
    
    // Recursive case - calls itself with modified parameter
    return recursiveFunction(modifiedParameter);
}
```
### What is currying?
**Currying** is a technique that transforms a function with multiple arguments into a sequence of functions, each taking a single argument. It allows you to partially apply arguments to a function.

#### Basic Concept:
```javascript
// Regular function
function add(a, b, c) {
    return a + b + c;
}

// Curried version
function curriedAdd(a) {
    return function(b) {
        return function(c) {
            return a + b + c;
        };
    };
}

// Usage
const result1 = add(1, 2, 3);           // 6
const result2 = curriedAdd(1)(2)(3);    // 6
```
#### Generic Currying Function:
```javascript
function curry(func) {
    return function curried(...args) {
        if (args.length >= func.length) {
            return func.apply(this, args);
        } else {
            return function(...nextArgs) {
                return curried.apply(this, args.concat(nextArgs));
            };
        }
    };
}

// Usage
const multiply = (a, b, c) => a * b * c;
const curriedMultiply = curry(multiply);

console.log(curriedMultiply(2)(3)(4));     // 24
console.log(curriedMultiply(2, 3)(4));     // 24
console.log(curriedMultiply(2)(3, 4));     // 24
```

### What is function composition?
**Function composition** is the process of combining two or more functions to create a new function. The output of one function becomes the input of the next function, creating a pipeline of transformations.

#### Basic Concept:
```javascript
// Individual functions
const addOne = x => x + 1;
const multiplyByTwo = x => x * 2;
const square = x => x * x;

// Manual composition
const result = square(multiplyByTwo(addOne(5)));
// 5 → 6 → 12 → 144
```

#### Generic Compose Function:
```javascript
// Right-to-left composition (traditional)
function compose(...fns) {
    return function(value) {
        return fns.reduceRight((acc, fn) => fn(acc), value);
    };
}

// Left-to-right composition (pipe)
function pipe(...fns) {
    return function(value) {
        return fns.reduce((acc, fn) => fn(acc), value);
    };
}

// Usage
const transform = compose(square, multiplyByTwo, addOne);
const result = transform(5); // 144

const transformPipe = pipe(addOne, multiplyByTwo, square);
const result2 = transformPipe(5); // 144
```

