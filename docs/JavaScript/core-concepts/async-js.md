---
sidebar_position: 5
---

# Async JS and Fetch API
Asynchronous JavaScript allows code to run without blocking the main thread—enabling non-blocking operations like network requests, timers, and file I/O. JavaScript is single-threaded: if one task takes too long (e.g. API call), it can freeze the whole app. Async lets JS handle long-running tasks in the background and react when they're done.

## Promises

### What are Promises?
A **Promise** is an object representing the eventual completion or failure of an asynchronous operation. It provides a cleaner way to handle asynchronous code compared to callbacks.

#### Promise States:
- **Pending** - Initial state, neither fulfilled nor rejected
- **Fulfilled** - Operation completed successfully
- **Rejected** - Operation failed

Once fulfilled or rejected, a promise becomes **settled** and cannot change again.

### How do you create a promise?
There are several ways to create a Promise in JavaScript, each suited for different scenarios.

#### 1. Using the Promise Constructor:
You create a Promise using the `new Promise` constructor, which takes an **executor function** with two parameters: `resolve` and `reject`.

```javascript
const myPromise = new Promise((resolve, reject) => {
    // Asynchronous operation
    setTimeout(() => {
        const success = Math.random() > 0.5;
        if (success) {
            resolve('Operation successful');
        } else {
            reject('Operation failed');
        }
    }, 1000);
});
```

#### 2. Promise.resolve() - Immediate Success:
```javascript
const resolvedPromise = Promise.resolve('Immediate value');
// Equivalent to:
const resolvedPromise = new Promise(resolve => resolve('Immediate value'));

resolvedPromise.then(value => console.log(value)); // "Immediate value"
```

#### 3. Promise.reject() - Immediate Failure:
```javascript
const rejectedPromise = Promise.reject('Immediate error');
// Equivalent to:
const rejectedPromise = new Promise((_, reject) => reject('Immediate error'));

rejectedPromise.catch(error => console.error(error)); // "Immediate error"
```

### What is async/await?
**async/await** is syntactic sugar built on top of Promises that makes asynchronous code look and behave more like synchronous code. It provides a cleaner way to write and handle asynchronous operations.

#### Basic Syntax:
```javascript
// async function declaration
async function fetchData() {
    // await pauses execution until Promise resolves
    const data = await fetch('/api/data');
    const result = await data.json();
    return result;
}

// async arrow function
const fetchData = async () => {
    const data = await fetch('/api/data');
    return await data.json();
};
```

#### How it Works:
```javascript
async function example() {
    console.log('1. Start');
    
    // This pauses execution until Promise resolves
    const result = await new Promise(resolve => {
        setTimeout(() => resolve('Async result'), 1000);
    });
    
    console.log('2. Result:', result);
    console.log('3. End');
}

example();
// Output: 1. Start → (1 second delay) → 2. Result: Async result → 3. End
```
:::warning
**`await` can only be used inside async functions** - except in top-level await (ES2022)
:::

:::info
- **async functions always return Promises** - even if you return a regular value
- **Error handling** - Use `try/catch` instead of `.catch()`
- **Performance** - Use `Promise.all()` for parallel operations
:::

### How are errors handled in async/await vs Promises?
Error handling in async/await uses `try/catch`, while in Promises it’s done with `.catch()`.
#### Promise Error Handling:
```javascript
function fetchDataPromise() {
    return fetch('/api/data')
        .then(response => {
            if (!response.ok) {
                throw new Error(`HTTP error! status: ${response.status}`);
            }
            return response.json();
        })
        .then(data => {
            console.log('Data:', data);
            return data;
        })
        .catch(error => {
            console.error('Error:', error);
            throw error; // Re-throw if needed
        });
}

// Usage
fetchDataPromise()
    .then(result => console.log('Success:', result))
    .catch(error => console.error('Failed:', error));
```

#### async/await Error Handling:
```javascript
async function fetchDataAsync() {
    try {
        const response = await fetch('/api/data');
        
        if (!response.ok) {
            throw new Error(`HTTP error! status: ${response.status}`);
        }
        
        const data = await response.json();
        console.log('Data:', data);
        return data;
    } catch (error) {
        console.error('Error:', error);
        throw error; // Re-throw if needed
    }
}

// Usage
fetchDataAsync()
    .then(result => console.log('Success:', result))
    .catch(error => console.error('Failed:', error));
```

### How does promise chaining work?
If each `.then()` returns a new promise, you can chain async steps. **Promise chaining** allows you to execute multiple asynchronous operations in sequence, where the output of one operation becomes the input of the next. 
#### Simple Example:
```javascript
fetch('/api/user/1')
    .then(response => response.json())
    .then(user => {
        console.log('User:', user);
        return fetch(`/api/posts/${user.id}`);
    })
    .then(response => response.json())
    .then(posts => console.log('Posts:', posts))
    .catch(error => console.error('Error:', error));
```

### How do `Promise.all`, `Promise.race`,` Promise.allSettled`, `Promise.any` work?
These static methods handle multiple Promises in different ways:

#### Promise.all() - All Must Succeed:
`Promise.all()` waits for all promises to succeed. Use when you want to run multiple async tasks in parallel and wait for all to finish successfully.
```javascript
const promises = [
    fetch('/api/users'),
    fetch('/api/posts'),
    fetch('/api/comments')
];

Promise.all(promises)
    .then(responses => {
        // All promises resolved successfully
        return Promise.all(responses.map(r => r.json()));
    })
    .then(data => console.log('All data:', data))
    .catch(error => {
        // If ANY promise fails, entire operation fails
        console.error('One failed:', error);
    });
```

#### Promise.race() - First One Wins:
`Promise.race()` returns the first settled promise (either resolved or rejected). Use when you want to get the result of the fastest promise — whether it resolves or rejects.
```javascript
const timeoutPromise = new Promise((_, reject) => {
    setTimeout(() => reject('Timeout'), 5000);
});

const dataPromise = fetch('/api/slow-data');

Promise.race([dataPromise, timeoutPromise])
    .then(result => console.log('First to complete:', result))
    .catch(error => console.error('Error:', error));
```

#### Promise.allSettled() - Wait for All (Success or Failure):
`Promise.allSettled()` - Waits for all promises to finish, regardless of success or failure.
```javascript
const promises = [
    fetch('/api/data1'),
    fetch('/api/data2'),
    fetch('/api/data3')
];

Promise.allSettled(promises)
    .then(results => {
        results.forEach((result, index) => {
            if (result.status === 'fulfilled') {
                console.log(`Promise ${index}:`, result.value);
            } else {
                console.log(`Promise ${index} failed:`, result.reason);
            }
        });
    });
```

#### Promise.any() - First Success Wins:
`Promise.any()` - First success wins, fails only if all fail
```javascript
const promises = [
    fetch('/api/primary'),
    fetch('/api/backup1'),
    fetch('/api/backup2')
];

Promise.any(promises)
    .then(response => {
        // First successful response
        console.log('Got data from:', response.url);
    })
    .catch(error => {
        // All promises failed
        console.error('All sources failed:', error);
    });
```

#### Common Use Cases:
- **Promise.all** - Loading multiple required resources
- **Promise.race** - Timeout handling
- **Promise.allSettled** - Batch operations where some may fail
- **Promise.any** - Fallback/backup systems

### What is async programming with callbacks?

Asynchronous programming with **callbacks** means passing a function (the callback) to be executed after an async operation completes (e.g., timers, I/O, network). Asynchronous callbacks are executed after a certain event or operation has been completed. They are non-blocking and allow the code execution to continue while waiting for the operation to finish.


#### Basic Example
```javascript
function fetchData(callback) {
  setTimeout(() => {
    const data = { name: 'John', age: 30 };
    callback(data);
  }, 1000);
}

fetchData((data) => {
  console.log(data);
});
```

#### Callback Hell (nesting)
“callback hell”, difficult error propagation, inversion of control.

```javascript
getUser(id, (err, user) => {
  if (err) return handle(err);
  getPosts(user.id, (err, posts) => {
    if (err) return handle(err);
    getComments(posts[0].id, (err, comments) => {
      if (err) return handle(err);
      console.log(comments);
    });
  });
});
```
### What is the difference between setTimeout(fn, 0) and Promise.resolve().then(fn)?
**setTimeout(fn, 0)** schedules `fn` as a **macrotask** (runs after current call stack and all microtasks).

**Promise.resolve().then(fn)** schedules `fn` as a **microtask** (runs before the next macrotask).

#### Example:
```javascript
console.log('1');

setTimeout(() => console.log('2'), 0);
Promise.resolve().then(() => console.log('3'));

console.log('4');

// Output: 1, 4, 3, 2
```
**Microtasks** (Promise.then) have higher priority than **macrotasks** (setTimeout)
- **setTimeout(fn, 0)** doesn't guarantee immediate execution

### What is asynchronous iteration (for await...of)?
**Asynchronous iteration** lets you iterate over data that arrives over time (e.g., streams, paginated APIs). It uses `for await...of` to await each value from an **async iterable**.

```javascript
const urls = [
  "https://api.example.com/user/1",
  "https://api.example.com/user/2"
];

async function fetchUsers() {
  for await (const url of urls) {
        const res = await fetch(url);
        const data = await res.json();
        console.log(data);
    }
}
```
#### Making an object async-iterable:
:::info
An object is async iterable if it has a `[Symbol.asyncIterator]()` method that returns an iterator with an async `next()` method.
:::

```javascript
const asyncRange = {
  from: 1, to: 3,
  async *[Symbol.asyncIterator]() {
    for (let i = this.from; i <= this.to; i++) {
      await new Promise(r => setTimeout(r, 100));
      yield i;
    }
  }
};

(async () => {
  for await (const n of asyncRange) {
    console.log(n); // 1,2,3 (with delays)
  }
})();
```

## Fetch API

### How does fetch() work?
`fetch(url, options?)` is a modern API for making HTTP requests. It returns a Promise that resolves to a `Response` object (rejects only on network errors).

#### Basic GET
```javascript
const res = await fetch('/api/users');
if (!res.ok) throw new Error(`HTTP ${res.status}`);
const data = await res.json(); // or res.text(), res.blob(), res.arrayBuffer()

fetch("https://api.example.com/users", {
  method: "POST",
  headers: {
    "Content-Type": "application/json"
  },
  body: JSON.stringify({ name: "Oleh", age: 25 })
})
  .then(res => res.json())
  .then(data => console.log("Created:", data))
  .catch(err => console.error("Error:", err));

```
:::tip
Always check `res.ok`/`res.status`; fetch resolves even for 4xx/5xx.
:::

### How do you set request headers?
Request headers are key–value pairs you include in your request to tell the server how to interpret the request or what additional info is included.

Use the `headers` option (an object or `Headers` instance) when calling `fetch`.

#### Basic
```javascript
await fetch('/api/data', {
  method: 'GET',
  headers: {
    'Accept': 'application/json',
    'Authorization': 'Bearer TOKEN'
  }
});
```
#### Using Headers object
```javascript
const headers = new Headers();
headers.set('Accept', 'application/json');
headers.set('X-Request-ID', '123');

await fetch('/api/data', { headers });
```

### How do you specify request methods and bodies?
By default, `fetch()` uses the GET method. To use another HTTP method (like POST, PUT, DELETE) use `method` and `body` options in the fetch options object.

#### POST with JSON
```javascript
await fetch('/api/users', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ name: 'John', email: 'john@example.com' })
});
```

### How to upload files with fetch?

Files are sent as `multipart/form-data`. Use `FormData` to upload files with fetch.
#### Single file upload
```javascript
const fileInput = document.getElementById('fileInput');
const file = fileInput.files[0];

const formData = new FormData();
formData.append('file', file);
formData.append('description', 'My file');

await fetch('/api/upload', {
  method: 'POST',
  body: formData // Content-Type set automatically
});
```
#### Multiple files
```javascript
const fileInput = document.getElementById('fileInput');
const files = fileInput.files;

const formData = new FormData();
Array.from(files).forEach((file, index) => {
  formData.append(`file${index}`, file);
});

await fetch('/api/upload', {
  method: 'POST',
  body: formData
});
```

### How to abort a request? (AbortController)
You can abort a `fetch()` request using an `AbortController`, which gives you a signal to cancel the request before it completes.
#### Basic abort
```javascript
const controller = new AbortController();

fetch('/api/slow-data', { signal: controller.signal })
  .then(response => response.json())
  .catch(error => {
    if (error.name === 'AbortError') {
      console.log('Request was aborted');
    }
  });

// Cancel the request
controller.abort();
```
### What are fetch arguments?

`fetch(url, options?)` takes two arguments:

#### 1. URL (required)
```javascript
fetch('/api/users');           // Relative URL
fetch('https://api.example.com/users'); // Absolute URL
```

#### 2. Options object (optional)
```javascript
fetch('/api/users', {
  method: 'POST',              // HTTP method (default: 'GET')
  headers: {                   // Request headers
    'Content-Type': 'application/json',
    'Authorization': 'Bearer token'
  },
  body: JSON.stringify(data),  // Request body
  credentials: 'include',      // Cookie policy: 'omit' | 'same-origin' | 'include'
  mode: 'cors',               // Request mode: 'cors' | 'no-cors' | 'same-origin'
  cache: 'no-cache',          // Cache policy
  redirect: 'follow',         // Redirect handling: 'follow' | 'error' | 'manual'
  referrer: 'https://example.com', // Referrer URL
  signal: controller.signal    // AbortController signal
});
```

### Why do we need to use res.json() and why is it async?
The `Response` object from fetch contains the raw response data as a **stream**. You need to call `.json()` to parse it into JavaScript objects.

#### Why res.json() is needed:
```javascript
const response = await fetch('/api/users');
console.log(response); // Response object, not the actual data

const data = await response.json(); // Parse the JSON stream
console.log(data); // Actual JavaScript object/array
```

#### Why it's async:
```javascript
// JSON parsing can be slow for large responses
const response = await fetch('/api/large-data'); // 10MB JSON file
const data = await response.json(); // Parsing happens asynchronously
```
:::warning
**Body can only be read once** - use `response.clone()` if needed twice
:::

### What are response properties methods?
The `Response` object has properties and methods to inspect and consume the response.

#### Properties:
```javascript
const response = await fetch('/api/users');

console.log(response.status);        // 200, 404, 500, etc.
console.log(response.statusText);    // "OK", "Not Found", etc.
console.log(response.ok);           // true if status 200-299
console.log(response.url);          // Final URL after redirects
console.log(response.type);         // "basic", "cors", "opaque"
console.log(response.headers);       // Headers object
```

#### Methods to read body:
```javascript
const response = await fetch('/api/data');

// Parse as different formats
const json = await response.json();        // JavaScript object
const text = await response.text();        // Plain text
const blob = await response.blob();        // Binary data
const arrayBuffer = await response.arrayBuffer(); // Raw bytes
const formData = await response.formData(); // Form data
```

#### Headers methods:
```javascript
const response = await fetch('/api/data');

// Check if header exists
if (response.headers.has('Content-Type')) {
  console.log('Has content type');
}

// Get header value
const contentType = response.headers.get('Content-Type');
console.log(contentType); // "application/json"

// Get all headers
for (const [key, value] of response.headers) {
  console.log(`${key}: ${value}`);
}
```