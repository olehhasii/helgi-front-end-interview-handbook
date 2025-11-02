---
sidebar_position: 9
---

# Modules & APIs

## Modules

### How do modules work in JS?
**Modules** in JavaScript allow you to **split your code into reusable files**. Each module can **export** variables, functions, or classes, and **import** them into other files. This promotes modularity, maintainability, and avoids polluting the global scope.

#### ES6 Modules (ESM)
With ES6, JavaScript introduced native modules — supported by modern browsers and Node.js (with `"type": "module"` in package.json). Each `.js` file is treated as a separate module, with its own scope.

1) **Named Exports**

Export multiple bindings (functions, variables, classes):
```javascript
// math.js
export const PI = 3.14159;
export function add(a, b) {
  return a + b;
}
export const subtract = (a, b) => a - b;
```
Import them in another module:
```javascript
// app.js
import { PI, add, subtract } from "./math.js";

console.log(add(2, 3)); // 5
console.log(PI);        // 3.14159
```
You can also rename imports/exports:
```javascript
import { add as sum } from "./math.js";
```
2) **Default Exports**
Each module can optionally export one default value — typically a function or class.

```javascript
// logger.js
export default function log(message) {
  console.log("Log:", message);
}
```
Import it without braces:
```javascript
// app.js
import log from "./logger.js";
log("Hello modules!");
```

:::info
Each module has its own private scope — variables and functions are not accessible globally unless explicitly exported.
```javascript
// inside utils.js
const secret = "hidden"; // private
export const visible = "exported"; // public
```
:::

#### 🧩 Before ES6: Global Scope and CommonJS
Before ES6 (2015), JavaScript didn’t have a native module system.
- In browsers, scripts shared the same **global scope**, which caused naming collisions.  
- In Node.js, the **CommonJS** system was introduced (`require`, `module.exports`).

**CommonJS example (Node.js):**

```javascript
// math.js
module.exports.add = (a, b) => a + b;

// app.js
const math = require("./math");
console.log(math.add(2, 3)); // 5
```
:::info
CommonJS is synchronous, meaning it loads modules at runtime (blocking).
:::

### What are dynamic imports?
**Dynamic imports** allow you to **load JavaScript modules at runtime** instead of at the start of execution. They use the `import()` function, which returns a **Promise** that resolves to the module object. This feature enables **lazy-loading**, **code-splitting**, and **conditional imports** — making applications faster and more modular. 

```javascript
async function loadModule() {
  const module = await import("./math.js");
  module.add(2, 3);
}

// or .then
import("./module.js")
  .then((module) => {
    module.doSomething();
  })
  .catch((err) => {
    console.error("Failed to load module:", err);
  });

if (user.isAdmin) {
  const adminTools = await import("./admin-tools.js");
  adminTools.init();
}
```

#### How It Works
- The `import()` expression is asynchronous — it doesn’t block execution.
- It returns a Promise that resolves to the module’s exports.
- Unlike static import statements, it can appear anywhere in code (inside functions, conditionals, loops, etc.).

### What is import.meta?
The **`import.meta`** object provides **metadata about the current module**. It’s a special built-in object available **only inside ES modules**, and it’s automatically created by the JavaScript engine.

Unlike normal objects, you **can’t create or override** it manually — it’s provided by the module system itself.


- `import.meta` - Object containing metadata about the current module
- `import.meta.url` -The full URL or file path to the current module
- `import.meta.env` - Build tool–specific environment info (Vite, Snowpack)
- `new URL(path, import.meta.url)` - Create absolute URLs relative to the module

## JavaScript APIs

### What is URL APIs?
The **URL API** in JavaScript provides built-in classes for **parsing, constructing, and manipulating URLs** safely and easily. It helps developers handle complex URL strings — such as query parameters, protocol, host, and paths — without manual string concatenation.

The main classes are:
- **`URL`** – represents and manipulates a full URL
- **`URLSearchParams`** – works with query string parameters

URL Properties:
- `href`	Full URL string	("https://example.com/path?x=1")
- `protocol`	Protocol used	("https:")
- `hostname`	Domain name	("example.com")
- `port`	Port number	("8080")
- `pathname`	Path portion	("/path/page.html")
- `search`	Query string (with ?)	("?name=Alice")
- `hash`	Fragment identifier (with #)	("#section1")
- `origin`	Combination of protocol + host	("https://example.com:8080")

```javascript
const url = new URL("https://example.com/products?category=books&page=2");
url.hostname;   // "example.com"
url.pathname;   // "/products"
url.search;     // "?category=books&page=2"
url.searchParams.get("category"); // "books"

const url = new URL("https://example.com");

url.pathname = "/users";
url.search = "?id=42";
url.hash = "#profile";

console.log(url.href); // "https://example.com/users?id=42#profile"
```

`URLSearchParams` provides methods to read, add, delete, or modify query string parameters.
```javascript
const params = new URLSearchParams("?name=Alice&age=25");

console.log(params.get("name"));   // "Alice"
console.log(params.has("age"));    // true
params.set("age", "26");
params.append("city", "Paris");
params.delete("name");

console.log(params.toString());    // "age=26&city=Paris"
```

:::tip
The `URL` object has a built-in `searchParams` property of this type:
```javascript
const url = new URL("https://example.com?x=10&y=20");
console.log(url.searchParams.get("x")); // "10"
```
:::

### What are storage APIs (localStorage, sessionStorage, cookies, IndexedDB)?
**Storage APIs** in JavaScript allow web applications to **store data on the client-side** (in the browser). They enable persistence across sessions or page reloads, improve performance, and support offline features.

The main browser storage mechanisms are:
- **localStorage**
- **sessionStorage**
- **Cookies**
- **IndexedDB**

#### 1. `localStorage`
`localStorage` provides **key–value storage** that persists **indefinitely** (until cleared by the user or script). It’s synchronous and can only store **strings**.
```javascript
// Store data
localStorage.setItem("theme", "dark");

// Retrieve data
const theme = localStorage.getItem("theme");

// Remove a specific key
localStorage.removeItem("theme");

// Clear everything
localStorage.clear();
```

#### 2. sessionStorage
`sessionStorage` works just like localStorage but with shorter lifespan — its data persists only within the current tab/session (data is cleared when tab or window closes.
).
```javascript
sessionStorage.setItem("auth", "temporaryToken");
console.log(sessionStorage.getItem("auth"));
```
#### 3. Cookies
Cookies are small pieces of data (≈4KB) stored in the browser and sent automatically with every HTTP request to the same domain. They are mainly used for: Authentication (sessions), tracking, server communication
Can be configured with:
- `max-age` or `expires` → duration
- `path`, `domain` → scope
- `Secure`, `HttpOnly`, `SameSite` → security flags

```javascript
document.cookie = "user=Oleh; path=/; max-age=3600";
```
`document.cookie` returns all cookies for the current domain as a single string.
```javascript
console.log(document.cookie); // "user=Alice; theme=dark; language=en"
```
You can parse it into an object:
```javascript
function getCookie(name) {
  return document.cookie
    .split("; ")
    .find(row => row.startsWith(name + "="))
    ?.split("=")[1];
}

console.log(getCookie("theme")); // "dark"
```
To delete a cookie, set its expiration date in the past:
```javascript
document.cookie = "user=; expires=Thu, 01 Jan 1970 00:00:00 GMT; path=/";
```

#### 4. IndexedDB
`IndexedDB` is a low-level NoSQL database built into the browser. It allows storing large structured data (objects, files, blobs) asynchronously.
```javascript
const request = indexedDB.open("myDatabase", 1);

request.onupgradeneeded = (event) => {
  const db = event.target.result;
  db.createObjectStore("users", { keyPath: "id" });
};

request.onsuccess = (event) => {
  const db = event.target.result;
  const tx = db.transaction("users", "readwrite");
  const store = tx.objectStore("users");
  store.put({ id: 1, name: "Alice" });
};
```

### What are Regular Expressions and the RegExp API?
**Regular Expressions (RegEx or RegExp)** are patterns used to **match, search, and manipulate strings** based on specific text rules.  

They’re supported natively in JavaScript through the **`RegExp` class** and built-in **string methods** like `.match()`, `.replace()`, `.search()`, and `.split()`.

You can create RegExp patterns in two ways:
```javascript
// 1. Literal notation
const pattern = /hello/i;

// 2. Constructor
const regex = new RegExp("hello", "i");
```
#### RegExp Methods:
- `test()` → returns true/false
- `exec(str)` → Returns match details (like match() but for RegExp)
- `lastIndex` → Tracks position for `global`/`sticky` matches

#### String Methods with RegEx Support
- `str.match(regex)` → returns matched substrings
- `str.replace(regex, repl)` → replaces matched text
- `str.split(regex)` → splits string by regex
- `str.search(regex)` → Returns index of first match
- `str.matchAll(regex)` → Returns iterator with all matches (ES2020)

#### Special Characters
- `.` → any character
 `\d` → digit (0–9)
- `\w` → word character
- `\s` → whitespace
- `^` → start of string
- `$` → end of string
- `+, *, ?` → quantifiers
- `[...]` - Character set
- `{n,m}` - Between n and m repetitions

#### Flags
- `g` → global (all matches)
- `i` → case-insensitive
- `m` → multiline

### What is the History API?
The **History API** is a browser feature that allows JavaScript to **manipulate the session history** — the list of pages visited in the current tab — without causing a full page reload.  

The History API is part of the `window.history` object, which gives access to the browser’s session history.

#### Navigation Methods
- `history.back()` - Goes back one page (same as browser “Back” button)
- `history.forward()` - Goes forward one page
- `history.go(n)` - Moves `n` steps in history (`n` can be negative)

#### Manipulating History Without Reload
Modern SPAs use `pushState()` and `replaceState()` to update the URL and browser history without reloading the page.
```javascript
// Adds a new entry to the history stack
history.pushState({ page: "about" }, "About", "/about");

// Replaces the current entry (no new history entry created)
history.replaceState({ page: "home" }, "Home", "/");
```
- The first argument (state) is an object associated with the entry.
- The second (title) is mostly ignored by browsers.
- The third (url) updates the address bar (must be same-origin).

### What are location and navigation APIs?
The **Location API** and **Navigation APIs** are part of the browser’s **Window** interface. They allow you to **inspect, modify, and control the current page’s URL** and handle navigation programmatically.

#### Location API (`window.location`)
The **Location API** represents the current URL of the browser window (or frame).
```javascript
console.log(window.location);
```
**Common Properties**
- `href` - Full URL	
- `protocol` - Protocol used	
- `host` - Host + port	
- `hostname` - Host only	
- `port` - Port number	
- `pathname` - Path of the URL	
- `search` - Query string (with ?)	
- `hash` - Fragment identifier (with #)	
- `origin` - Protocol + host

**Common Methods**
- `assign(url)` - Navigate to a new page (adds to history)
- `replace(url)` - Navigate to a new page (replaces history entry)
- `reload(force)` - Reloads the current page

#### Navigation API (`window.navigation`)
The `window.navigation` object provides access to the Navigation API, which enables listening to and controlling all navigation events (reloads, back/forward, history updates).

#### Key Methods
- `navigate(url)` - Programmatically navigate to a new URL (like `location.assign`)
- `reload()` - Reloads current page
- `back() / forward()` - Navigate through history entries
- `transition` - Used for SPA-style smooth transitions

### What is Performance API (e.g., Performance.now(), requestIdleCallback)?
The **Performance API** provides a set of tools to **measure, analyze, and optimize the performance** of web applications. It helps developers track timing, measure execution duration, and schedule non-critical tasks efficiently.

The `performance` object provides methods and properties to measure timing and performance metrics.

- `performance.now()` returns a high-resolution timestamp (in milliseconds, with sub-millisecond precision). It measures time relative to the page load, not the UNIX epoch like `Date.now()`.
```javascript
const start = performance.now();

// Code to measure
for (let i = 0; i < 1e6; i++) {}

const end = performance.now();
console.log(`Execution time: ${end - start} ms`);
```
- `performance.mark()` and `performance.measure()` - used for creating custom performance metrics.
```javascript
performance.mark("start-task");

// Simulate work
doHeavyWork();

performance.mark("end-task");
performance.measure("task-duration", "start-task", "end-task");

const measure = performance.getEntriesByName("task-duration")[0];
console.log(measure.duration, "ms");
```

- The `PerformanceObserver` API. The `PerformanceObserver` interface listens for real-time performance events like layout shifts, paints, or long tasks.
```javascript
const observer = new PerformanceObserver((list) => {
  for (const entry of list.getEntries()) {
    console.log(entry.name, entry.duration);
  }
});

observer.observe({ entryTypes: ["measure", "paint", "longtask"] });
```
- `requestIdleCallback()` lets the browser run non-critical code when the main thread is idle — helping improve responsiveness by deferring background tasks. The callback runs when the browser is not busy rendering or handling input.
```javascript
requestIdleCallback((deadline) => {
  while (deadline.timeRemaining() > 0) {
    console.log("Doing background work...");
  }
});
```

### What is the Internationalization API?
The **Internationalization API (`Intl`)** provides built-in JavaScript tools for **locale-sensitive formatting** of:
- Numbers
- Dates and times
- Currencies
- Lists
- Relative times (e.g., “3 minutes ago”)
- Plural rules and collations

It contains constructors for various formatters:
| Formatter                 | Purpose                                      |
| :------------------------ | :------------------------------------------- |
| `Intl.NumberFormat`       | Format numbers, currencies, and percentages  |
| `Intl.DateTimeFormat`     | Format dates and times                       |
| `Intl.Collator`           | Compare strings in locale-aware way          |
| `Intl.ListFormat`         | Join lists with locale rules (“A, B, and C”) |
| `Intl.RelativeTimeFormat` | Display relative times (“5 minutes ago”)     |
| `Intl.PluralRules`        | Determine pluralization forms                |
| `Intl.DisplayNames`       | Display language, region, or currency names  |

```javascript
const price = new Intl.NumberFormat("ja-JP", {
  style: "currency", currency: "JPY"
}).format(2500);

const date = new Intl.DateTimeFormat("ja-JP", {
  dateStyle: "medium"
}).format(new Date());

console.log(`価格: ${price} — 日付: ${date}`);
// "価格: ￥2,500 — 日付: 2025/10/23"
```