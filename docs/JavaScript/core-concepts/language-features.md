---
sidebar_position: 7
---

# Language Features (Set/Map, Timers, Statements, Expressions)

### What are timers?
Timers in JavaScript are functions that schedule code execution after a delay or at intervals. They’re provided by the browser (Web APIs) or Node.js, not the JS engine itself.

#### 🧩 Common Timer Functions

| Function | Description | Returns |
|:----------|:-------------|:----------|
| `setTimeout(callback, delay, ...args)` | Executes a function once after the given delay (in ms). | A numeric timer ID |
| `clearTimeout(id)` | Cancels a timeout scheduled with `setTimeout`. | — |
| `setInterval(callback, delay, ...args)` | Repeatedly calls a function with a fixed time delay between each call. | A numeric interval ID |
| `clearInterval(id)` | Cancels an interval scheduled with `setInterval`. | — |
| `setImmediate(callback)` *(Node.js only)* | Executes a callback immediately after the current poll phase (faster than `setTimeout(..., 0)`). | Immediate object |
| `process.nextTick(callback)` *(Node.js only)* | Executes callback **before** the next event loop tick (higher priority than `Promise`). | — |

---

#### ⚙️ Example – `setTimeout` and `setInterval`

```javascript
console.log("Start");

setTimeout(() => {
  console.log("Executed after 1 second");
}, 1000);

const id = setInterval(() => {
  console.log("Repeating every second");
}, 1000);

setTimeout(() => {
  clearInterval(id); // stop after 3 seconds
  console.log("Interval cleared");
}, 3000);

console.log("End");
```

### What is Set?
A **`Set`** in JavaScript is a **built-in collection** that stores **unique values** — no duplicates are allowed. It’s part of the ES6 (ES2015) standard and is similar to an array, but optimized for **fast membership checks** and **uniqueness**. Can hold any type: primitives or object references. Insertion order is preserved when iterating.

#### Common methods:

- `add(value)` → add a value.
- `has(value)` → check existence.
- `delete(value)` → remove value.
- `clear()` → remove all values.
- `size` → number of elements.

```javascript
const set = new Set();
set.add(1);
set.add(2);
set.add(2); // duplicate, ignored
console.log(set); // Set(2) { 1, 2 }

const a = new Set([1, 2, 3]);
const b = new Set([3, 4, 5]);

// Union
const union = new Set([...a, ...b]); // {1, 2, 3, 4, 5}

// Intersection
const intersection = new Set([...a].filter(x => b.has(x))); // {3}

// Difference
const difference = new Set([...a].filter(x => !b.has(x))); // {1, 2}

```
### What are Maps?
A Map in JavaScript is a collection of key–value pairs where keys can be of any type (not just strings like in plain objects). Unlike regular objects, `Map` allows **any type of value** (objects, functions, primitives) to be used as **keys**, and maintains the **insertion order** of its elements.

#### Common methods:
- `set(key, value)` → add/update a pair.
- `get(key)` → retrieve a value.
- `has(key)` → check if a key exists.
- `delete(key)` → remove a key.
- `clear()` → remove all entries.
- `size` → number of entries.

#### 🧩 Creating a Map

```javascript
const map = new Map();

// Add values
map.set("name", "Alice");
map.set("age", 25);
map.set(true, "boolean key");
map.set({ id: 1 }, "object key");

console.log(map);
// Map(4) { "name" => "Alice", "age" => 25, true => "boolean key", {id:1} => "object key" }

```
Or initialize directly from an array of key–value pairs:
```javascript
const map = new Map([
  ["name", "Bob"],
  ["age", 30],
]);
```

### What are expressions and statements?
JavaScript program consists of a sequence of **statements**. Each **statement** is an instruction to do something, like create a variable, run an `if/else` condition, or start a loop.
An **expression** is *any piece of code that evaluates to a value*. It can be used anywhere a value is expected — assigned to a variable, passed as an argument, or returned from a function.

- **Expressions** → *produce a value*  
- **Statements** → *perform an action*

```javascript
5 + 10           // expression → evaluates to 15
"Hello".toUpperCase()  // expression → "HELLO"
a && b           // expression → value depends on a, b
x > 0            // expression → true or false

let x = 10;          // variable declaration statement
if (x > 5) { ... }   // if statement
for (let i = 0; i < 3; i++) { ... } // for loop statement
return x;            // return statement
break;               // break statement

```

:::tip
Want to know whether a chunk of JS is an expression or a statement? Try to log it out!
:::

### What are conditional statements?
Conditional statements let you execute different blocks of code depending on whether a condition is true or false.

#### 🧩 `if`, `else if`, and `else`
The `if` statement tests a condition and runs its block if the condition is `true`. You can optionally use `else` and `else if` to handle other cases.

```javascript
const score = 85;

if (score >= 90) {
  console.log("Excellent");
} else if (score >= 70) {
  console.log("Good");
} else {
  console.log("Needs improvement");
}
```
#### `switch` Statement
The switch statement is useful when comparing the same expression against multiple possible values. The `break` keyword stops further case execution. Without `break`, control “falls through” to the next case.
```javascript
const fruit = "apple";

switch (fruit) {
  case "apple":
    console.log("It's red!");
    break;
  case "banana":
    console.log("It's yellow!");
    break;
  default:
    console.log("Unknown fruit");
}
```
#### Ternary Operator `(? :)`
A compact way to write simple conditional expressions. The ternary operator is an expression, not a statement — it produces a value.

#### Short-Circuit Evaluation
Logical operators `&&` and `||` can act as shorthand conditionals.

```javascript
const isAdmin = true;
isAdmin && console.log("Welcome admin"); // executes if true

const user = null;
const name = user || "Guest"; // "Guest"
```

### What are loops (for, while, for...of, etc.)?
**Loops** allow you to execute a block of code **multiple times** — typically while iterating over data structures or repeating an action until a condition is met.

JavaScript provides several types of loops, each suited to different situations.

#### `for` Loop
The classic `for` loop runs as long as its **condition** is true. It’s often used when the number of iterations is known in advance.

```javascript
for (initialization; condition; update) {
  // code to execute
}
```
- Initialization – executed once before the loop starts.
- Condition – checked before each iteration.
- Update – runs after each iteration.

#### `while` Loop
Runs a block as long as a condition is true. Use it when the number of iterations isn’t known beforehand.

```javascript
let count = 0;

while (count < 3) {
  console.log(count);
  count++;
}
```

:::warning
If the condition is initially false, the loop body never runs.
:::

#### `do...while` Loop
Similar to while, but the loop body runs at least once, because the condition is checked after execution.
```javascript
let i = 0;

do {
  console.log(i);
  i++;
} while (i < 3);
```

#### `for...of` Loop
Used to iterate over iterable objects like `arrays`, `strings`, `sets`, or `maps`. Provides the values directly.

```javascript
const fruits = ["apple", "banana", "orange"];

for (const fruit of fruits) {
  console.log(fruit);
}
```

#### `for...in` Loop
Iterates over enumerable property keys of an object.

```javascript
const user = { name: "Alice", age: 25 };

for (const key in user) {
  console.log(key, user[key]);
}
```

:::warning
for...in is meant for objects, not arrays. It iterates over all enumerable keys (including inherited ones), which can lead to unexpected results with arrays.
:::

### What are jump statements (break, continue, return, throw)?
**Jump statements** alter the normal flow of execution in a program. They allow you to **exit loops**, **skip iterations**, **return values from functions**, or **raise errors**.
- `break` immediately **terminates the nearest loop or `switch` statement**, and transfers control to the statement after it.
- `continue` skips the rest of the current iteration of a loop and moves directly to the next one.
- `return` exits the current function immediately and optionally provides a value to the caller.
- `throw` is used to raise (throw) an exception — an error that interrupts normal execution. You can throw any value (object, string, number), but typically an Error object.

### What is 'use strict'?
`'use strict'` is a directive that enables Strict Mode, which makes JavaScript execution more secure and less error-prone by enforcing stricter rules.
- Prevents accidental globals
- Silent errors become visible: E.g., assigning to read-only properties throws an error.
- ES6 modules (import/export) are strict by default.

### What is a destructuring assignment?
**Destructuring assignment** is an ES6 feature that lets you **extract values from arrays or properties from objects** and assign them to variables in a **concise, declarative way**. It improves readability and reduces repetitive code when dealing with structured data.

#### 🧩 Array Destructuring
Destructuring arrays assigns elements based on their position (index order):

```javascript
const numbers = [10, 20, 30];

const [a, b, c] = numbers;
console.log(a, b, c); // 10 20 30
```
You can skip elements using commas:
```javascript
const [first, , third] = [1, 2, 3];
console.log(first, third); // 1 3
```
You can also use default values:
```javascript
const [x = 1, y = 2] = [10];
console.log(x, y); // 10 2
```
And use rest syntax to collect remaining elements:
```javascript
const [head, ...tail] = [1, 2, 3, 4];
console.log(head); // 1
console.log(tail); // [2, 3, 4]
```
#### Object Destructuring
Destructuring objects assigns variables based on property names, not order.
```javascript
const user = { name: "Alice", age: 25 };

const { name, age } = user;
console.log(name, age); // Alice 25
```

You can rename variables during destructuring and set default values for missing properties:
```javascript
const { name: userName, age: userAge } = user;
console.log(userName, userAge); // Alice 25

const { role = "guest" } = user;
console.log(role); // guest
```

You can destructure nested arrays and objects directly:
```javascript
const person = {
  name: "Bob",
  address: {
    city: "Paris",
    zip: 75000
  }
};

const {
  name,
  address: { city }
} = person;

console.log(name, city); // Bob Paris
```

### How does JS handle dates and times?
JavaScript uses the built-in `Date` object to handle dates and times. Internally, JS dates are stored as milliseconds since Unix epoch (Jan 1, 1970 UTC)

You can create `Date` objects in several ways:

```javascript
// Current date and time
const now = new Date();

// From a timestamp (milliseconds since epoch)
const fromTimestamp = new Date(0); // Jan 1, 1970 UTC

// From a date string
const fromString = new Date("2024-05-15T10:00:00Z");

// From individual components
const custom = new Date(2024, 4, 15, 10, 30); // May 15 2024, 10:30 (months are 0-indexed)
```
:::info
Months in JavaScript’s Date constructor are 0-indexed (0 = January, 11 = December).
:::

#### Getting Date and Time Components
`Date` objects provide many getters for individual parts:

```javascript
const date = new Date("2024-05-15T10:30:45Z");

console.log(date.getFullYear()); // 2024
console.log(date.getMonth());    // 4 (May)
console.log(date.getDate());     // 15
console.log(date.getDay());      // 3 (Wednesday)
console.log(date.getHours());    // depends on local time zone
console.log(date.getMinutes());  // 30
console.log(date.getSeconds());  // 45
```
There are also UTC versions of all these methods (`getUTCFullYear()`, etc.), which return values in Coordinated Universal Time instead of local time.

#### Setting Date Components
You can modify existing `Date` objects with setter methods:
```javascript
const date = new Date();
date.setFullYear(2030);
date.setMonth(11);   // December
date.setDate(25);
console.log(date.toString());
```

#### Comparing Dates
Because `Date` objects can be converted to timestamps, you can compare them directly using relational operators. 

You can get or compare times numerically using timestamps (milliseconds). Or using `date.getTime()`:

```javascript
const start = Date.now(); // current time in ms

// Simulate delay
for (let i = 0; i < 1e6; i++) {}

const end = Date.now();
console.log(`Elapsed: ${end - start} ms`);

const date = new Date();
console.log(date.getTime()); // milliseconds since Jan 1, 1970 UTC

const d1 = new Date("2024-05-15");
const d2 = new Date("2024-06-01");

console.log(d1 < d2); // true
console.log(+d1);     // numeric timestamp
```

#### Time Zones and UTC
The `Date` object automatically uses the user’s local time zone for display. To handle times consistently, prefer UTC methods or ISO strings:
```javascript
const date = new Date("2024-05-15T12:00:00Z");
console.log(date.toISOString());   // 2024-05-15T12:00:00.000Z
console.log(date.toUTCString());   // Wed, 15 May 2024 12:00:00 GMT
console.log(date.toLocaleString()); // Localized format (depends on user)
```

### What is the Error class?
The **`Error`** class in JavaScript is the **base type for all runtime errors**. It represents an error that occurs during program execution and provides useful information for debugging — such as the message and stack trace.

#### Properties of the Error Object
- `name` -	The type or name of the error	("Error", "TypeError")
- `message` -	Description of the error	("File not found")
- `stack` -	Stack trace showing where the error occurred	

#### Built-in Error Types
- `Error`	Generic base error
- `ReferenceError`	Accessing a variable that doesn’t exist
- `TypeError`	Using a value in an invalid way (e.g., calling something that’s not a function)
- `SyntaxError`	Invalid JavaScript syntax (usually thrown at parse time)
- `RangeError`	A numeric value is outside an allowed range
- `EvalError`	Improper use of eval() (rare)
- `URIError`	Malformed encodeURI() or decodeURI() input
- `AggregateError`	Thrown by Promise.any() when all promises fail

:::tip
You can create your own error types by extending the `Error` class — useful for domain-specific errors.
```javascript
class ValidationError extends Error {
  constructor(message) {
    super(message);
    this.name = "ValidationError";
  }
}

try {
  throw new ValidationError("Invalid email format");
} catch (e) {
  console.log(e.name);    // "ValidationError"
  console.log(e.message); // "Invalid email format"
}
```
:::

### What is the `global` object?
The global object is the top-level object that provides access to built-in functions, values, and any variables declared in the global scope.
- Browser: `window` (or self, frames)
- Node.js: global
- Universal (ES2020+): `globalThis`

What it Contains:
- Built-in objects: `Math`, `JSON`, `Date`, etc.
- Functions: `setTimeout()`, `isNaN()`, etc.
- Global variables you declare (without `let`/`const`/`var` in strict mode this throws an error).
