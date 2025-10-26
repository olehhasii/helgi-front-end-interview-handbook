---
sidebar_position: 1
---

# General and fundamental concepts of JS

## JavaScript Basics and Internals

### What is JavaScript?
JavaScript (JS) is a high-level, interpreted, dynamic, single-threaded programming language primarily used to create interactive behavior on web pages. 

### What are ES5, ES6+, and ESNext?
ES5 and ES6+ are versions of ECMAScript, the standardized specification of JavaScript. ESNext refers to the latest features currently being added to the language.

### What is a JS Engine?
A JavaScript engine is a program that executes JavaScript code — for example, **V8** (Chrome, Node.js), **SpiderMonkey** (Firefox), or **JavaScriptCore** (Safari). It parses, compiles, and optimizes JS for execution. JS is Interpreted or just-in-time compiled. It means that the entire code is converted into machine code at once, then executed immediately.

### What is a JS Runtime?
A runtime provides the **environment** where JS code executes — it includes:
- The JS engine
- APIs (like DOM, `setTimeout`, or `fetch`)
- Event loop and callback queue  
- Call Stack: Keeps track of the execution context (function calls). Heap: A memory space used for storing objects. 

For example, Node.js runtime includes file system and network APIs, while browsers provide DOM and Web APIs.

### What is the Call Stack and how does it work?
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

### What is the Event Loop? How does it work?
The **Event Loop** is the core mechanism that allows JavaScript — a **single-threaded** language — to handle **asynchronous operations** without blocking the main thread. It handles asynchronous code by constantly checking the **call stack** and **task queues** (mircor/macro tasks), executing tasks when the stack is empty.
#### How the Event Loop Works:
1. **Call Stack** — place where JavaScript keeps track of the execution of functions. When a function is called, it’s added to the call stack, and when the function finishes execution, it is removed from the stack.
2. **Macrotask Queue** — place where asynchronous tasks (like setTimeout callbacks, I/O operations, or DOM events) wait to be executed.
3. **Microtask Queue** — microtask queue is for microtasks like `Promises`, `queueMicrotask()`, `process.nextTick()`, `MutationObserver` callbacks. Microtasks have higher priority, so JS processes all microtasks before any next macrotask.
4. **Event Loop** — continuously checks:
   - *Is the call stack empty?*
   - *If yes, take the next task from the microtask queue and push it to the stack.*
   - *Once the microtask queue is empty, it takes the first task from the macrotask queue and pushes it onto the call stack for execution.*

### What are Microtasks and Macrotasks?
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

### How does the event loop works with rendering (e.g., requestAnimationFrame)?
The browser event loop manages both JavaScript tasks and rendering. Rendering only happens between tasks, never in the middle of one. `requestAnimationFrame` schedules a callback to run right before the next repaint, making it ideal for smooth animations.

## Types, Values, Equality, Operators

### What are JavaScript data types?
JavaScript has 8 Data Types: 7 Primitive and 1 Reference type (`Object`).
#### Primitive Types:
- **Number**: Floating point numbers. Used for decimals and integers
- **String**: Sequence of characters. Used for text
- **Boolean**: Logical type that can only be true or false
- **Undefined**: Variable that has been declared but not assigned a value
- **Null**: Represents an intentional absence of any object value
- **Symbol**: Unique and immutable primitive value (ES6+)
- **BigInt**: Arbitrary precision integers (ES2020+)
#### Reference Type:
- **Object**: Collection of key-value pairs. Includes arrays, functions, dates, etc.

#### Examples:
```javascript
// Primitive types
let num = 42;                    // Number
let text = "Hello";              // String
let flag = true;                 // Boolean
let notDefined;                  // Undefined
let empty = null;                // Null
let id = Symbol('id');           // Symbol
let bigNum = 123n;              // BigInt

// Reference type
let person = { name: "John" };   // Object
let arr = [1, 2, 3];            // Array (type of Object)
let func = function() {};        // Function (type of Object)
```

### What’s the difference between primitive types and reference types?
- **Primitive** types: stores actual values, they are immutable and compared by value.
- **Reference** types (objects, arrays, functions): store memory addresses (references) to objects, they are mutable and compared by reference.

#### Examples of Non-primitive (Reference) data types:
- **Object**: Used to store collections of data.
- **Array**: An ordered collection of data.
- **Function**: A callable object.
- **Date**: Represents dates and times.
- **RegExp**: Represents regular expressions.
- **Map**: A collection of keyed data items.
- **Set**: A collection of unique values.

### What is the difference between null, undefined and Undeclared ?
- **Undefined**: means a variable was declared but not assigned value. 
- **Null**: Explicitly set by the developer to indicate that a variable has no value
- **Undeclared**:  variables are created when you assign a value to an identifier that is not previously created using `var`, `let` or `const`. Undeclared variables will be defined globally, outside of the current scope. In strict mode, a `ReferenceError` will be thrown when you try to assign to an undeclared variable.

```javascript
function foo() {
  x = 1; // Throws a ReferenceError in strict mode
}
```

:::info

- typeof undefined → "undefined"
- typeof null → "object" (legacy behavior)

:::

### What are truthy and falsy values?
Falsy Values (evaluated as false in boolean context):
- `false`
- `0`
- `""` (empty string)
- `null`
- `undefined`
- `NaN`

Everything else is truthy.

:::info[Converting to Boolean:]
```javascript
// Using Boolean() constructor
Boolean(0);        // false
Boolean("");       // false
Boolean("hello");  // true
Boolean(42);       // true

// Using double negation (!!)
!!0;               // false
!!"";              // false
!!"hello";         // true
!!42;              // true
```
:::

### What’s the difference between == and ===?
- `==` (loose equality): Converts operands to the same type before comparison.
- `===` (strict equality): Compares both type and value exactly. The === operator will not do type conversion, so if two values are not the same type === will simply return false

### What is type conversion vs coercion?
- Conversion (explicit): You do it intentionally using functions like `Number()`, `String()`, `Boolean()`.
- Coercion (implicit): JS does it automatically in expressions (especially with `+`, `==,` or logical operators).

### What are Symbols? When do you use them?

A Symbol is a unique and immutable primitive value used to create hidden or unique property keys in objects.
Symbols are created with `Symbol()` and guarantee uniqueness—even if two symbols have the same description. They're often used to avoid property name clashes or to define "private-like" properties. Even if another dev uses `Symbol('id')`, it won't match yours.

```javascript
const sym = Symbol('id');
const user = { [sym]: 123 };
```

#### Common Use Cases:

**1. Avoiding property name conflicts:**
```javascript
const id = Symbol('id');
const user = {
    [id]: 123,
    name: 'John'
};
// Won't conflict with other libraries using 'id'
```

**2. Defining constants like `Symbol.iterator` for custom iteration behavior:**
```javascript
const obj = {
    [Symbol.iterator]: function* () {
        yield 1;
        yield 2;
        yield 3;
    }
};
// Makes object iterable
```

**3. Private-like properties:**
```javascript
const _private = Symbol('private');
class MyClass {
    constructor() {
        this[_private] = 'secret data';
    }
}
```

#### Key Points:
- Symbols are **not enumerable** in `for...in` loops
- Use `Object.getOwnPropertySymbols()` to access them
- Each `Symbol()` call creates a **unique** symbol
- `Symbol.for('key')` creates **global** symbols


### How does object-to-primitive conversion work?
When an object is used in a context where a primitive is expected (`string`, `number`, `boolean`), JavaScript tries to convert it using specific rules:
1. **String context** → `toString()` → `valueOf()`
2. **Number context** → `valueOf()` → `toString()`
3. **Default context** → `valueOf()` → `toString()`
#### Examples:
```javascript
const obj = {
    valueOf() { return 42; },
    toString() { return "hello"; }
};

// String context
console.log(String(obj));        // "hello"
console.log(obj + "");          // "hello"

// Number context  
console.log(Number(obj));       // 42
console.log(+obj);              // 42

// Default context (usually string)
console.log(obj == "hello");  // true
```

:::danger
Custom `toString()` or `valueOf()` can cause confusing results if misused.
:::

### What are unary, binary, and ternary operators?
- **Unary**: takes one operand (e.g., `-x`, `!x`, `typeof x`).
- **Binary**: takes two operands (e.g., `x + y`, `x > y`, `x && y`).
- **Ternary**: takes three operands; the only one in JS is the conditional operator: `condition ? value1 : value2`.

### What is operator precedence in JS?
Operator precedence defines the order in which different operators are evaluated in an expression. Higher-precedence operators run before lower-precedence ones, unless parentheses change the order.

#### Example: 
`2 + 3 * 4` → multiplication runs first → result 14.

Parentheses () always have the highest precedence and override defaults.

When operators have the same precedence, associativity decides the direction (e.g., * is left-to-right, assignment = is right-to-left).

### What is the spread and rest operators?
- **Spread** `...` expands elements from arrays/objects. It's used to unpack values:
    - In arrays: `const newArr = [...arr1, ...arr2]`
    - In objects: `const copy = { ...obj }`
    - In function calls: `func(...args)`
- **Rest** `...` collects multiple elements into an array or object. It's used to collect values:
    - In functions: `function sum(...nums) {}`
    - In destructuring:
    ```javascript
    const [first, ...rest] = [1, 2, 3];     // rest = [2, 3]
    const { a, ...others } = { a: 1, b: 2 } // others = { b: 2 }
    ```

### What are floating-point rounding errors in JS?

JavaScript uses **IEEE 754 double-precision floating-point** format, which can cause precision issues with decimal arithmetic.

#### The Problem:
```javascript
console.log(0.1 + 0.2);        // 0.30000000000000004
console.log(0.3 === 0.1 + 0.2); // false
console.log(0.1 + 0.2 === 0.3); // false
```

#### Why This Happens:
- Computers represent numbers in **binary** (base 2)
- Some decimal numbers can't be exactly represented in binary
- `0.1` in binary is `0.00011001100110011...` (repeating)
- This causes tiny rounding errors

#### Solutions:

**1. Round to specific decimal places:**
```javascript
const result = 0.1 + 0.2;
console.log(Math.round(result * 100) / 100); // 0.3
```

**2. Use Number.EPSILON for comparison:**
```javascript
function isEqual(a, b) {
    return Math.abs(a - b) < Number.EPSILON;
}
console.log(isEqual(0.1 + 0.2, 0.3)); // true
```

### Why are strings immutable in JS?
Strings are primitives, not objects — they don’t have mutable memory like arrays. Any operation that looks like it modifies a string actually creates a new one. Operations like `toUpperCase()` return a new string. Immutability makes strings safe to share across code without unexpected side effects.

### What is the difference between string primitives and String objects?
A string primitive is a basic immutable value like `"hello"`. A String object is created with the `new String("hello")` constructor and is a wrapper object around the primitive. JS automatically wraps a primitive in a temporary String object when you call a method (`"hi".toUpperCase()` works because of this).

### What is string interning?
String interning is a memory optimization where identical string values are stored only once and reused. In JavaScript, many engines keep a string pool so that identical literals point to the same memory.


## Variables & Scope

### What are the differences between var, let, and const?
`var`, `let`, and `const` declare variables, but differ in scope, hoisting, and mutability.

- `var`:
    - Function-scoped
    - Hoisted (but initialized as undefined)
    - Can be redeclared
    - Avoid in modern code
- `let`:
    - Block-scoped
    - Hoisted (but not initialized → Temporal Dead Zone)
    - Cannot be redeclared in the same scope
    - Preferred for reassignable variables
- `const`:
    - Block-scoped
    - Hoisted (same TDZ behavior as let)
    - Must be initialized at declaration
    - Value can’t be reassigned (but object contents can mutate)

### What is scope?
Scope is the area in code where variables are visible and accessible. There is different kind of scopes:
- **Global scope**: variables are accessible everywhere.
- **Function scope**: variables declared inside a `function` are visible only inside it.
- **Block scope**: variables declared with `let` and `const` are limited to `{}` blocks.

### What is hoisting?
Hoisting means variable/function declarations are moved to the top of their scope. JavaScript hoists declarations (not initializations).
- `var` is hoisted and initialized as undefined
- `let` and `const` are hoisted but not initialized
- Function declarations are hoisted entirely (with body)

### What is lexical scope?
Lexical scope means that a function's scope is determined by where it's **written** in the code, not where it's **called**. Inner functions always “see” variables from their parent scopes.
#### How it works:
```javascript
const global = "I'm global";

function outer() {
    const outerVar = "I'm in outer";
    
    function inner() {
        console.log(outerVar);  // Can access outer's variables
        console.log(global);    // Can access global variables
    }
    
    return inner;
}

const myFunction = outer();
myFunction(); // Still has access to outer's variables
```

### What is the Temporal Dead Zone (TDZ)?
TDZ (Temporal Dead Zone) is the time between entering a block and initializing a let/const variable, during which access causes an error. From block start until initialization, accessing the variable throws a ReferenceError.
```javascript
console.log(a); // undefined (hoisted var)
var a = 5;
console.log(b); // ❌ ReferenceError (TDZ)
let b = 10;
```
### What is the scope chain?
When a variable is not found in the local scope, JS searches up the chain: JS checks `inner` → `outer` → `global` for variables.


