---
sidebar_position: 1
---

# React General and Core concepts

### What is React?
**React** is an **open-source JavaScript library** created by **Meta (Facebook)** for building **user interfaces (UIs)** — especially **single-page applications (SPAs)** that update dynamically without reloading the page.

React’s main concept is the **Component** — an independent, reusable piece of UI that manages its own logic and rendering.

Key Features

- **Declarative UI**: You describe what the UI should look like, and React takes care of updating it efficiently.
- **Virtual DOM**: React maintains an in-memory “virtual” representation of the DOM. When state changes, it compares differences and updates only what’s necessary — improving performance.
- **Component-Based**: UI is divided into independent, reusable parts. Each component can have its own logic and style.
- **Unidirectional Data Flow**: Data flows from parent to child via props, making apps easier to reason about.
- **JSX Syntax**: Combines HTML-like syntax with JavaScript, allowing logic and UI to coexist clearly.
- **Hooks** (from React 16.8): Functions like useState, useEffect, etc., let you use state and lifecycle features in functional components.

### Why you use React instead of vanilla Javascript or Jquery?
With plain JavaScript, we write codes to manipulate the DOM directly. In plain JavaScript UI is created in HTML on the server side, which is then sent to the browser. Manipulating DOM with Plain JavaScript includes selecting HTML elements, updating their properties and handling user interactions manually using event listeners. With plain JavaScript we can fully control the DOM but it can easily become complex, unmanageable and time-consuming, mainly for large scale applications.

Whereas, React.js:
- **Component-Based Architecture**: React's component-based architecture allows us to break down User Interface elements into chunks or smaller and reusable components.
- **Better Code Organization and Maintance**: With Component based architecture, where we break down UI elements to smaller and reusable components, managing our web application become more easy and it leads to a better code organization.
- **JSX**: React makes use of JSX(JavaScript XML), a syntax extension for JavaScript that allows us to write HTML within JavaScript files.
- **DOM Manipulation**: React uses Virtual DOM. In Virtuall DOM only the parts that are changed gets updated. This optimization improves performance and a smooth user experience.
- **State Management**: React offers robust state management system, that makes it easy to manage and update the state of UI components.

### Advantages/Disadvanages of React

#### Advantages of React
| Advantage                        | Description                                                                                                      |
| -------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| **Declarative Syntax**           | React focuses on *what the UI should look like*, while vanilla JS/jQuery focuses on *how to manipulate the DOM*. |
| **Component-Based Architecture** | Encourages reusability, modularity, and separation of concerns.                                                  |
| **Virtual DOM**                  | Improves performance by updating only the parts of the DOM that change.                                          |
| **State Management**             | Simplifies tracking and reacting to data changes without manual DOM handling.                                    |
| **Ecosystem & Tools**            | Large community, dev tools, hooks, testing utilities, and libraries (React Router, Redux, etc.).                 |
| **Cross-Platform**               | Works not only for web (React DOM) but also for mobile apps via React Native.                                    |
| **Maintainability**              | Easier to scale and debug large applications with predictable data flow.                                         |


#### Disadvanages of React
| Disadvantage                      | Description                                                                     |
| --------------------------------- | ------------------------------------------------------------------------------- |
| **Initial Learning Curve**        | JSX, hooks, and component lifecycle can be confusing for beginners.             |
| **Build Setup**                   | Requires bundlers (Vite, Webpack) — unlike simple script tags with jQuery.      |
| **Frequent Updates**              | Ecosystem evolves fast — developers must keep up with changes.                  |
| **Overkill for Small Projects**   | For static or minimal-interaction pages, vanilla JS or jQuery might be simpler. |
| **SEO Limitations (without SSR)** | Client-side rendering can affect SEO unless you use frameworks like Next.js.    |

### Declarative React
**Declarative React** means describing *what the UI should look like* for a given state — instead of writing step-by-step instructions for *how to update the UI*. React takes care of rendering the interface and keeping it in sync with your data automatically.

In **imperative** programming, you give explicit instructions on how things should happen. DOM APIs are inherently imperative. When manipulating the DOM, this often means selecting elements and modifying them manually.

**Declarative** programming, on the other hand, focuses on describing the desired outcome rather than detailing how to achieve it. React components allow us to declare what the UI should look like, and React takes care of updating it when state changes.

```javascript
// Imperative approach (Vanilla JS / jQuery):**
const button = document.querySelector("#btn");
const text = document.querySelector("#text");
let count = 0;

button.addEventListener("click", () => {
  count++;
  text.textContent = `Clicked ${count} times`;
});


// Declarative approach (React):
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <button onClick={() => setCount(count + 1)}>
      Clicked {count} times
    </button>
  );
}
```

### What is Component?
A **component** is a **self-contained, reusable piece of UI** that defines both how something looks and how it behaves. In React and other modern frameworks, components are the **building blocks** of an application.

**React components** are JavaScript functions that return markup. A component is a piece of the UI (user interface) that has its own logic and appearance.

```jsx
function Greeting({ name }) {
  return <h1>Hello, {name}!</h1>;
}
```

:::warning
React components are regular JavaScript functions, but their names must start with a capital letter or they won’t work!
:::

### What Is a Root Component File?
The **root component** (often named `App.js` or `App.jsx`) is the **entry point** of your React application's **component tree**. It acts as the **top-level container** that holds all other components.

### Function Components VS Class Components
In React, components can be defined in **two main ways** — as **function components** or **class components**.  

#### 1. Function Components
**Function components** are simple JavaScript functions that return JSX.

They are **stateless by default**, but can use **Hooks** (like `useState`, `useEffect`, etc.) to handle state and side effects.

**Example:**
```jsx
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <button onClick={() => setCount(count + 1)}>
      Count: {count}
    </button>
  );
}
```
#### 2. Class Components 
Class components are ES6 classes that extend `React.Component`.

They use `this.state` for local state and lifecycle methods (e.g., `componentDidMount`, `componentDidUpdate`, `componentWillUnmount`).

### Components Lifecycle
A **component lifecycle** describes the **sequence of events** that occur from the **creation** of a React component to its **removal** from the DOM.  

**Lifecycle** is divided into three main phases: Mounting, Updating, and Unmounting

1. **Mounting** - A component **mounts** when it’s added to the screen.
2. **Updating** - A component **updates** when it receives new props or state, usually in response to an interaction.
3. **Unmounting** - A component **unmounts** when it’s removed from the screen.

#### Class Component Lifecycle
React class components have **built-in lifecycle methods** grouped into three main phases:

##### Mounting Phase (Component Creation)
This phase occurs when a component is first created and inserted into the DOM. Key lifecycle methods (for class components) in this phase include: 
| Method | Description |
|---------|--------------|
| **constructor()** | Initializes state and binds methods. |
| **static getDerivedStateFromProps()** | Updates state based on props before render. |
| **render()** | Returns JSX to render the UI. |
| **componentDidMount()** | Runs after the component is mounted (perfect for API calls or subscriptions). |

##### Updating Phase (Component Re-render)
This phase occurs when a component's state or props change, leading to a re-render. Key lifecycle methods (for class components) in this phase include:
| Method                                | Description                                                  |
| ------------------------------------- | ------------------------------------------------------------ |
| **static getDerivedStateFromProps()** | Sync state with updated props.                               |
| **shouldComponentUpdate()**           | Controls whether a re-render should occur.                   |
| **render()**                          | Renders new UI.                                              |
| **getSnapshotBeforeUpdate()**         | Captures info before DOM updates (e.g., scroll position).    |
| **componentDidUpdate()**              | Invoked after re-rendering — useful for reacting to changes. |

##### Unmounting Phase (Component Removal)
This phase occurs when a component is removed from the DOM. 
| Method                     | Description                                                      |
| -------------------------- | ---------------------------------------------------------------- |
| **componentWillUnmount()** | Cleanup method (remove event listeners, cancel API calls, etc.). |

#### Function Component Lifecycle (Hooks)
Function components don’t have built-in lifecycle methods — instead, they use Hooks like `useEffect` to replicate the same behavior.

**`useEffect` Lifecycle Equivalents**
| Lifecycle                | Hook Equivalent                     | Description                      |
| ------------------------ | ----------------------------------- | -------------------------------- |
| **componentDidMount**    | `useEffect(() => {...}, [])`        | Runs once after component mounts |
| **componentDidUpdate**   | `useEffect(() => {...}, [dep])`     | Runs when a dependency changes   |
| **componentWillUnmount** | `return cleanup` inside `useEffect` | Runs before unmount for cleanup  |

### What is the difference between React Node, React Element, and a React Component?

#### React Node
A **React Node** is **anything that can be rendered** by React.

It’s the **broadest type** — it represents all possible return values React can handle inside JSX or a component’s `render()`.

**A React Node can be:**
- A **React element** (`<div />`)
- A **string** (`"Hello"`)
- A **number** (`42`)
- **null** or **undefined**
- A **boolean** (`true`/`false` are ignored)
- An **array** or **fragment** of nodes
- A **portal** or another component’s rendered output

#### React Element
A **React Element** is a plain JavaScript object that describes what you want to see on the screen. It is created by **JSX** or by calling `React.createElement()`.

React uses these elements to build and update the Virtual DOM.

Example:
```jsx
const element = <h1 className="title">Hello</h1>;

const element = React.createElement("h1", { className: "title" }, "Hello");
```

The above `element` is an object, not an actual DOM node.

It looks like:
```javascript
{
  type: "h1",
  props: { className: "title", children: "Hello" }
}
```

#### React Component
A React Component is a **function** or **class** that returns a React Element (or tree of elements).

Components are the building blocks of React apps — they describe how sections of the UI should look and behave.

### What is JSX?
**JSX (JavaScript XML)** is a **syntax extension** for JavaScript that allows you to write **HTML-like code** inside your React components. 

JSX looks like HTML, but it’s actually **syntactic sugar** for `React.createElement()` calls.

**Example (JSX):**
```jsx
const element = <h1 className="title">Hello, React!</h1>;
```
Under the hood, React compiles this to:
```jsx
const element = React.createElement("h1", { className: "title" }, "Hello, React!");
```
> JSX is not HTML — it’s a JavaScript expression that produces a React element.

Why JSX Is Useful:
- Makes UI code declarative and familiar (similar to HTML).
- Keeps logic and layout together — improving readability.
- Enables JavaScript expressions directly inside markup.
- Reduces boilerplate compared to manual createElement() calls.

JSX Rules to Remember:
1. JSX is compiled — React apps must go through a build tool (like Babel or Vite).
2. Expressions only, not statements (e.g., you can’t use if directly).
3. Keys are required when rendering lists for efficient updates.
4. Self-closing tags must be closed (`<img />`, `<input />`, etc.).
5. JSX Must Have a Single Root Element - JSX expressions must return one parent element. If you need multiple elements, wrap them in a fragment (`<>...</>`) or a `div`.

### What rendering means in React?
In React, rendering means converting React elements (JS objects) into actual DOM elements that the user sees on the screen — or re-rendering them when state or props change.

It’s how React converts your component logic (JSX + state + props) into **DOM updates** that the browser can display.

### How rendering works in React?
Rendering a component in React involves several steps, from writing JSX to updating the DOM

#### 1. Initial Render (Mounting Phase)
1. The root component (usually `<App />`) is **mounted** via:
   ```jsx
   const root = ReactDOM.createRoot(document.getElementById('root'));
   root.render(<App />);
   ```
2. React calls each component function (or class `render()` method).
3. Each component returns JSX, which React compiles into React elements.
4. React builds the Virtual DOM — a lightweight JS object representing the UI.
5. React converts this Virtual DOM into real DOM nodes and attaches them to the page.

#### 2. Re-rendering (Updating Phase)
When **state** or **props** change, React re-renders affected components.

1. React Calls the Component Again - React re-executes the function component.
2. It's compiled into a new Virtual DOM tree
3. Virtual DOM Diffing (Reconciliation) - React compares the new virtual DOM tree with the previous virtual DOM. React uses its diffing algorithm to detect exactly what changed.
4. Commit Phase (DOM Update) - React now performs actual updates to the real DOM: Only changed elements (added, removed, different HTML attributes) are updated in the real DOM. Runs layout effects (`useLayoutEffect`) and side effects (`useEffect`).
5. Browser Paint - Finally, the browser re-renders the changed pixels on the screen.

### What triggers re-renders?
1. **State Changes (`useState`, `useReducer`)** - When a component’s **state** is updated, React re-renders it to reflect new data.
:::tip
React does not re-render if the new state value is identical to the previous one.
:::
2. **Parent Component re-render** - Even if the child’s logic doesn’t change, it re-renders by default when its parent re-renders — unless optimized with `React.memo`. If a parent component passes new props (different values or references), the child component will re-render. 
3. **Context Changes** - When the value provided by a React Context changes, all components consuming that context will re-render.

### What is virtual DOM?
The **Virtual DOM (VDOM)** is a **lightweight in-memory representation** of the actual DOM. It allows React to **update the user interface efficiently** by minimizing direct manipulations of the real DOM, which are typically slow.
- The **real DOM** represents the page structure in the browser.  
- The **Virtual DOM** is a **JavaScript object tree** that mirrors the real DOM structure.  
- React uses it to **compare UI changes** and **update only what’s necessary**.

Instead of changing the browser DOM directly (which is slow), React:
- Keeps a copy of the UI in memory (the virtual DOM).
- Updates that copy first.
- Efficiently syncs only the changed parts to the real DOM.

### How does React's reconciliation algorithm work?
The **reconciliation algorithm** is how React determines **what has changed** in the user interface and **updates only the necessary parts** of the DOM efficiently.  

**Reconciliation** is React’s process of comparing:
- the **previous Virtual DOM tree** (old UI), and  
- the **new Virtual DOM tree** (new UI after state or prop change),

to decide **which parts of the real DOM need to be updated**.

#### Step-by-Step Process
1. **Component Re-renders** - Whenever a component’s **state**, **props**, or **context** changes:
    - React calls the component again.
    - The component returns **new JSX**, which React turns into a **new Virtual DOM tree**.
2. **Virtual DOM Comparison (Diffing)**: React compares the **new Virtual DOM** to the **previous one** node by node. React uses **optimized heuristics** to make diffing O(n).
3. **Heuristics Used by React**: 
    - If the element type changes, React **destroys the old subtree** and **builds a new one**.
    **Example:**
    ```jsx
    // Old render
    <div>Hello</div>

    // New render
    <span>Hello</span>
    ```
    → React removes the `<div>` and creates a new `<span>` from scratch.
    - Elements of the same type get compared deeply: If the element type is the same, React keeps the existing DOM node and updates its attributes.
    **Example:**
    ```jsx
    // Old render
    - <button class="red">Click</button>
    // New render
    + <button class="blue">Click me</button>
    ```
    - If text content changes: → Update the text node directly
4. **The Commit Phase**: After diffing, React applies all necessary changes to the real DOM.

### Why Should You Use a `key` Prop When Rendering a List?
In React, the **`key` prop** is used to **uniquely identify elements** in a list. It helps React efficiently determine **which items have changed, been added, or removed** between renders — making the **reconciliation process faster and more accurate**.

- Keys must be unique among siblings. However, it’s okay to use the same keys for JSX nodes in different arrays.
- Keys must not change or that defeats their purpose! Don’t generate them while rendering.

If each `<li>` has a key:
- React reorders existing DOM nodes instead of recreating them.
- Only necessary updates are made.

#### Common Mistake — Using Index as Key
This works only if the list never changes order or length. If items are added, removed, or reordered, index keys break the mapping, causing:
- Incorrect element updates.
- Lost input focus or animations.
- Unnecessary re-renders.

### What are React Fragments used for?
A React Fragment lets you group multiple elements without adding an extra DOM node (like a `<div>`). It’s a lightweight wrapper used to return multiple children from a component.

### What is React Fiber?
**React Fiber** is the **core reconciliation engine** introduced in React 16. 

**React Fiber** was created to:
- Breaking rendering into **smaller units of work** (called *fibers*).  
- Making rendering **interruptible and resumable**.  
- Allowing React to **prioritize important updates** (like user input) over less important ones (like background data).

> 🧠 In short: Fiber allows React to perform rendering *incrementally* and *asynchronously*.

#### What Is a “Fiber”?
A **Fiber** is a **JavaScript object** that represents a **unit of work** — essentially one React component (or element) in the tree.

Each fiber node contains:
- Component **type** and **props**  
- A reference to its **parent, child, and sibling**  
- Information about **pending work**  
- The **state** of the component  
- A **link to the previous version** (for comparison)

### What are Pure Components?
A **Pure Component** in React is a component that **renders the same output** for the same **props and state** — it doesn’t re-render unless its data actually changes. 

React’s rendering process must always be pure. Components should only return their JSX, and not change any objects or variables that existed before rendering — that would make them impure!

In React, side effects usually belong inside event handlers. Event handlers are functions that React runs when you perform some action—for example, when you click a button. Even though event handlers are defined inside your component, they don’t run during rendering! So event handlers don’t need to be pure.

If you’ve exhausted all other options and can’t find the right event handler for your side effect, you can still attach it to your returned JSX with a useEffect call in your component. This tells React to execute it later, after rendering, when side effects are allowed. However, this approach should be your last resort.

### What is React strict mode and what are its benefits?
**React Strict Mode** is a **development-only feature** that helps you identify potential problems in your React application.  

When developing in **“Strict Mode”**, React calls each component’s function twice, which can help surface mistakes caused by impure functions. React calls certain functions twice in development (like component initialization and useEffect cleanup) to help detect impure or unsafe side effects.

| Benefit                               | Description                                                              |
| ------------------------------------- | ------------------------------------------------------------------------ |
| **Early bug detection**               | Warns about potential issues before they cause runtime errors.           |
| **Encourages best practices**         | Highlights deprecated APIs and unsafe patterns.                          |
| **Prepares for concurrent rendering** | Ensures components are compatible with React’s modern architecture.      |
| **No runtime cost**                   | Affects only development, not production performance.                    |
| **Safer effects**                     | Detects side effects that could cause inconsistencies during re-renders. |

### Dependency Arrays and Stale Closures in React
When using **React Hooks** like `useEffect`, `useCallback`, or `useMemo`, the **dependency array** plays a key role in controlling **when** your effect or memoized value runs or updates. Incorrect dependency arrays can lead to **stale closures** — a common source of subtle bugs in React apps.

#### What Is a Dependency Array?
A **dependency array** tells React **when to re-run** an effect or recalculate a memoized value.

```jsx
useEffect(() => {
  // effect logic
}, [dependencies]);
```
React compares the dependencies from the previous render to the current render:
- If any dependency changed → the effect runs again.
- If none changed → React skips re-running it.

#### What Is a Stale Closure?
A stale closure happens when a function inside a hook (like useEffect) captures outdated values from previous renders because the dependency array is missing necessary variables.

Example of a Stale Closure:
```jsx
function Timer() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const id = setInterval(() => {
      // ❌ 'count' here is always 0 (stale value)
      setCount(count + 1);
    }, 1000);

    return () => clearInterval(id);
  }, []); // count is missing from dependencies

  return <p>{count}</p>;
}
```
Because count isn’t in the dependency array, the callback "captures" the initial `count` (0) forever → `setCount(count + 1)` always updates from that old closure.

### Data Fetching in React

#### 1. Fetching Data with `useEffect`
The most common way to handle asynchronous data loading in React is by using the `useEffect` and `useState` hooks. `useEffect` allows you to perform side effects, such as data fetching, and `useState` helps manage the component's state.

**Example:**
```jsx
import { useEffect, useState } from "react";

function UsersList() {
  const [users, setUsers] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    async function fetchUsers() {
      try {
        const response = await fetch("https://jsonplaceholder.typicode.com/users");
        if (!response.ok) throw new Error("Network error");
        const data = await response.json();
        setUsers(data);
      } catch (err) {
        setError(err.message);
      } finally {
        setLoading(false);
      }
    }

    fetchUsers();
  }, []); // Empty array → runs once on mount

  if (loading) return <p>Loading...</p>;
  if (error) return <p>Error: {error}</p>;

  return (
    <ul>
      {users.map(user => <li key={user.id}>{user.name}</li>)}
    </ul>
  );
}
```

To avoid repeating fetch logic across components, create a **custom hook**.
```jsx
function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const controller = new AbortController();

    async function fetchData() {
      try {
        const res = await fetch(url, { signal: controller.signal });
        if (!res.ok) throw new Error("Failed to fetch");
        const json = await res.json();
        setData(json);
      } catch (err) {
        if (err.name !== "AbortError") setError(err);
      } finally {
        setLoading(false);
      }
    }

    fetchData();
    return () => controller.abort();
  }, [url]);

  return { data, loading, error };
}
```

#### 2. Using Data Fetching Libraries
For complex apps, manual fetching can get messy — libraries like React Query, SWR, or Apollo Client (GraphQL) handle caching, retries, and refetching automatically.

#### 3. Data Fetching in Server-Side Frameworks
Server-side data fetching is a technique where data is retrieved from a database or API before rendering the page on the server (SSR), rather than fetching it on the client after the initial load. SSR improves performance, SEO, and user experience by delivering pre-rendered, data-filled pages to the client.

Example (Next.js 14 Server Component):
```jsx
async function Page() {
  const res = await fetch("https://api.example.com/posts", { cache: "no-store" });
  const posts = await res.json();

  return (
    <ul>
      {posts.map(p => <li key={p.id}>{p.title}</li>)}
    </ul>
  );
}
```

### How do you cancel an API request if a component unmounts?
When performing **data fetching** in React, it’s important to **cancel ongoing requests** if a component **unmounts** before the request completes. Otherwise, React might try to **update state on an unmounted component**, which can cause memory leaks and warnings.

When a component unmounts:
- The network request may still be running.
- The response may return **after** the component is gone.
- If your fetch logic calls `setState()`, React warns because the component no longer exists.

#### 1. Using `AbortController` (Native Fetch API)
The **Fetch API** supports cancellation using the **`AbortController`** object.

```jsx
import { useEffect, useState } from "react";

function Users() {
  const [users, setUsers] = useState([]);
  const [error, setError] = useState(null);

  useEffect(() => {
    const controller = new AbortController(); // Create controller
    const signal = controller.signal;

    fetch("https://jsonplaceholder.typicode.com/users", { signal })
      .then(res => {
        if (!res.ok) throw new Error("Network error");
        return res.json();
      })
      .then(data => setUsers(data))
      .catch(err => {
        if (err.name !== "AbortError") setError(err); // Ignore abort errors
      });

    // Cleanup → cancels the request if the component unmounts
    return () => {
      controller.abort();
      console.log("Request aborted");
    };
  }, []);

  if (error) return <p>Error: {error.message}</p>;
  return (
    <ul>
      {users.map(u => <li key={u.id}>{u.name}</li>)}
    </ul>
  );
}
```

### How to Implement Debounce in API Calls — and Why
**Debouncing** is a performance optimization technique used to **limit how often a function executes** — especially useful for **API calls triggered by user input** (like search boxes). It ensures that your app doesn’t make a new request **on every keystroke**, but only **after the user stops typing** for a short delay.

Without debouncing:
- Each key press triggers a new API call.
- Causes **excessive network traffic**.
- Wastes bandwidth and may overload servers.
- Creates **race conditions** (outdated results arriving last).

With debouncing:
- The function runs **only once after a delay** since the last user input.
- Reduces API calls and improves performance.
- Makes the UI feel smoother and more efficient.

Debounce with `setTimeout` and `useEffect` is a **simple and effective approach for API-based searches.**

```jsx
import { useState, useEffect } from "react";

function Search() {
  const [query, setQuery] = useState("");
  const [results, setResults] = useState([]);

  useEffect(() => {
    if (!query) return; // Skip empty input

    const timeoutId = setTimeout(() => {
      fetch(`https://api.example.com/search?q=${query}`)
        .then(res => res.json())
        .then(data => setResults(data))
        .catch(err => console.error(err));
    }, 500); // 500ms debounce delay

    return () => clearTimeout(timeoutId); // Cleanup on next keystroke
  }, [query]);

  return (
    <div>
      <input
        type="text"
        value={query}
        placeholder="Search..."
        onChange={e => setQuery(e.target.value)}
      />
      <ul>
        {results.map(item => <li key={item.id}>{item.name}</li>)}
      </ul>
    </div>
  );
}
```

### What Are React Server Components?
**React Server Components (RSC)** are a powerful feature that allows you to **render components on the server** instead of the client. They enable better **performance**, **smaller bundle sizes**, and **faster page loads** by offloading non-interactive work to the server.

With **Server Components**, part (or all) of your component tree can be **rendered on the server**, and only the **necessary HTML and minimal data** are sent to the client.

#### Limitations
- Only supported in React 18+ (and frameworks like Next.js 13+).
- Server Components cannot:
  - Use hooks like useState, useEffect, or useRef.
  - Access browser-only APIs (e.g., window, document).

### What are Server Functions?
**Server Functions** allow Client Components to call async functions executed on the server. They allow you to **run server-side code directly from your React components**, without creating separate API routes or endpoints.

Traditionally, when a user submits data (like a form), you:
1. Send a request to an API route (`/api/...`).
2. The API runs on the server and updates the database.
3. The client fetches updated data again.

With **Server Actions**, you can **skip the API route entirely** — the client calls the server function directly.

Server Components can define Server Functions with the `"use server"` directive:
```jsx
// Server Component
import Button from './Button';

function EmptyNote () {
  async function createNoteAction() {
    // Server Function
    'use server';
    
    await db.notes.create();
  }

  return <Button onClick={createNoteAction}/>;
}
```

### What is React Compiler?
The **React Compiler** (introduced with React 19) is a new, **automatic optimization system** that makes React apps **faster by default** — without requiring manual memoization (`useMemo`, `useCallback`, `React.memo`). It automatically **analyzes and optimizes your components** during build time to prevent unnecessary re-renders and improve performance.

The compiler analyzes your component tree at build time and:
1. **Understands dependencies** (which values affect what).  
2. **Automatically skips re-renders** when inputs (props, state, context) haven’t changed.  
3. **Memoizes results and closures** behind the scenes.  
4. **Generates optimized code** for rendering and reconciliation.