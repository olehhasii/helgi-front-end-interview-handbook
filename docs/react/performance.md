---
sidebar_position: 7
---

# Performance and Otimizations in React

### Common Pitfalls in React

1. Unnecessary Re-renders - React re-renders components whenever their **props, state, or context** change. However, not all of those changes are meaningful — and unnecessary re-renders can slow your app.
Fix with `useMemo`, `useCallback`, `React.memo`
2. Stale Closures - A stale closure happens when a function “remembers” outdated variables because it was created during a previous render.
3. Missing Dependencies in useEffect - React’s useEffect runs based on its dependency array. Leaving out dependencies causes stale data, while adding unnecessary ones causes infinite loops.
4. Large Lists Without Virtualization - Rendering huge lists (hundreds or thousands of items) can cause slow initial render and scroll lag.

### Race Conditions in React
A **race condition** occurs when **multiple asynchronous operations compete** to update state, and the **final result depends on which one finishes last** — not necessarily the one that should win.

> 🧠 **In short:**  
> A race condition happens when **an outdated async call finishes after a newer one** and incorrectly updates the UI.

#### Fix #1 — Track the Latest Request
Use a flag or request ID to ignore outdated responses.
```jsx
function UserProfile({ userId }) {
  const [user, setUser] = useState(null);

  useEffect(() => {
    let isActive = true;

    fetch(`/api/user/${userId}`)
      .then((res) => res.json())
      .then((data) => {
        if (isActive) setUser(data); // only update if still relevant
      });

    return () => {
      isActive = false; // cancel outdated request
    };
  }, [userId]);

  return <div>{user ? user.name : "Loading..."}</div>;
}
```
#### Fix #2 — AbortController (Built-in Cancellation)
Modern browsers provide AbortController, allowing you to cancel fetch requests directly.
```jsx
function UserProfile({ userId }) {
  const [user, setUser] = useState(null);

  useEffect(() => {
    const controller = new AbortController();

    fetch(`/api/user/${userId}`, { signal: controller.signal })
      .then((res) => res.json())
      .then(setUser)
      .catch((err) => {
        if (err.name !== "AbortError") console.error(err);
      });

    return () => controller.abort(); // cancel on cleanup
  }, [userId]);

  return <div>{user ? user.name : "Loading..."}</div>;
}
```

#### Fix #3 — Using a Library (React Query / SWR)
Libraries like React Query or SWR handle cancellation and request deduplication automatically.

#### Fix #4 — Debounce Rapid Async Triggers
Debouncing prevents multiple requests in quick succession.

```jsx
const debouncedSearch = useCallback(
  debounce((query) => fetchResults(query), 300),
  []
);
```
Only the last call (after 300ms idle time) executes, avoiding unnecessary concurrent requests. 

### Waterfalls
A **waterfall** in React refers to a performance issue where **asynchronous operations run sequentially**, even though they **could (and should)** run in parallel.  

This usually happens when **data fetching or effect dependencies are chained incorrectly**, causing one request to wait for another unnecessarily.



#### The Classic Example
```jsx
async function fetchUserAndPosts(userId) {
  const user = await fetch(`/api/users/${userId}`).then((r) => r.json());
  const posts = await fetch(`/api/users/${userId}/posts`).then((r) => r.json());
  return { user, posts };
}
```
Here, the second fetch waits for the first one to finish — even though both could run independently.

#### Fix: Fetch in Parallel (Use `Promise.all`)
```jsx
function Profile({ userId }) {
  const [data, setData] = useState({ user: null, posts: null });

  useEffect(() => {
    Promise.all([
      fetch(`/api/users/${userId}`).then((r) => r.json()),
      fetch(`/api/users/${userId}/posts`).then((r) => r.json()),
    ]).then(([user, posts]) => setData({ user, posts }));
  }, [userId]);
}
```

### Memoization in React
**Memoization** is an optimization technique used in React to **avoid unnecessary recalculations or re-renders** by **caching the results** of expensive operations or component renders.

React provides several built-in tools for memoization: `useMemo`, `useCallback`, and `React.memo`.

### Lazy Loading in React
**Lazy loading** is a performance optimization technique that delays loading parts of your app — such as components, images, or data — **until they are actually needed**.  

In React, it’s commonly used for **code-splitting**: loading components **on demand** instead of bundling everything at once.

#### Why Lazy Loading?
By default, React bundles your entire app into one large JavaScript file. For small apps, that’s fine — but in large projects, it increases:
- ⏳ **Initial load time**
- 💾 **Bundle size**
- 🧭 **Time-to-interactive**

✅ Lazy loading splits your app into **smaller chunks** that are loaded dynamically.

#### React’s Built-in Lazy Loading API
React provides `React.lazy()` and `<Suspense>` for **component-level lazy loading**.

```jsx
import React, { Suspense } from "react";

const UserProfile = React.lazy(() => import("./UserProfile"));

function App() {
  return (
    <Suspense fallback={<p>Loading...</p>}>
      <UserProfile />
    </Suspense>
  );
}
```

1. React.lazy() dynamically imports the component.
2. `<Suspense>` shows the fallback (e.g., “Loading…”) while loading.
3. Once loaded, React renders the component normally.

When using routing (like React Router), load pages only when the route is visited.
```jsx
import { lazy, Suspense } from "react";
import { BrowserRouter, Routes, Route } from "react-router-dom";

const Home = lazy(() => import("./pages/Home"));
const About = lazy(() => import("./pages/About"));

function App() {
  return (
    <BrowserRouter>
      <Suspense fallback={<p>Loading page...</p>}>
        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="/about" element={<About />} />
        </Routes>
      </Suspense>
    </BrowserRouter>
  );
}
```

#### Lazy Loading Non-Component Resources
You can also lazy-load:
- Images (load when visible)
- Data (fetch on demand)
- Libraries (import only when required)

### Suspense
**React Suspense** is a mechanism for **handling asynchronous operations** (like lazy loading components or data) in a **declarative** way — by letting React “pause” rendering while waiting for something, and show a **fallback UI** in the meantime.

Traditionally, asynchronous logic (fetching, lazy imports, etc.) was handled **manually**:
```jsx
if (isLoading) return <Spinner />;
if (error) return <Error />;
return <DataView />;
```

Suspense allows React to manage waiting states automatically, centralizing all “loading” UI handling inside a `<Suspense>` boundary.
```jsx
import React, { Suspense } from "react";

const Profile = React.lazy(() => import("./Profile"));

function App() {
  return (
    <Suspense fallback={<p>Loading profile...</p>}>
      <Profile />
    </Suspense>
  );
}
```


React 18 expanded Suspense to work with asynchronous data fetching — not just lazy components. You can use the `use()` hook (or libraries like React Query / Relay) to suspend rendering while waiting for data.

```jsx
const userPromise = fetch("/api/user").then((res) => res.json());

function UserProfile() {
  const user = use(userPromise); // Suspends until data is resolved
  return <h2>{user.name}</h2>;
}

function App() {
  return (
    <Suspense fallback={<p>Loading user...</p>}>
      <UserProfile />
    </Suspense>
  );
}
```
React will pause rendering of UserProfile until the promise resolves — then continue rendering automatically.

:::warning
When using React.lazy() or data-fetching with Suspense, you must combine Suspense and ErrorBoundary to handle both “loading” and “failure” states.
:::

### Error Handling and Fallback UI Strategies in React
React provides declarative ways to **catch, handle, and recover from errors** —  using **Error Boundaries**, **fallback UIs**, and **graceful degradation** strategies.

#### Error Boundaries (React’s Built-in Mechanism)
**Error Boundaries** are React components that catch JavaScript errors **anywhere in their child tree** during rendering, lifecycle methods, or constructors.

:::warning
Error boundaries do not catch errors for:
- Event handlers
- Server side rendering
- Errors thrown in the error boundary itself (rather than its children)
- Asynchronous code (e.g. `setTimeout` or `requestAnimationFrame` callbacks); an exception is the usage of the startTransition function returned by the `useTransition` Hook. Errors thrown inside the transition function are caught by error boundaries 
:::

```jsx
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError() {
    return { hasError: true };
  }

  componentDidCatch(error, info) {
    console.error("Caught error:", error, info);
  }

  render() {
    if (this.state.hasError) {
      return <p>Something went wrong. Please try again.</p>;
    }
    return this.props.children;
  }
}
```

:::tip
There is currently no way to write an Error Boundary as a function component. However, you don’t have to write the Error Boundary class yourself. For example, you can use `react-error-boundary` lib instead.
:::

#### Handling Async or Event Errors
React Error Boundaries don’t catch async errors — you need to handle those manually.
```jsx
function UserProfile({ userId }) {
  const [user, setUser] = useState(null);
  const [error, setError] = useState(null);

  useEffect(() => {
    fetch(`/api/user/${userId}`)
      .then((res) => {
        if (!res.ok) throw new Error("Failed to load user");
        return res.json();
      })
      .then(setUser)
      .catch(setError);
  }, [userId]);

  if (error) return <p>Error: {error.message}</p>;
  if (!user) return <p>Loading...</p>;
  return <h2>{user.name}</h2>;
}
```

### List Virtualization
**List virtualization** (also called **windowing**) is a performance optimization technique that helps render **only visible items** in large lists — instead of rendering **every item** in the DOM.

This drastically improves **render speed**, **memory usage**, and **scroll performance** when displaying thousands of list items.

When you render a large list — say, 10,000 items — React tries to create and manage 10,000 DOM nodes at once.

Instead of rendering the full list, you render only the items currently visible in the viewport (and a small buffer above and below).

Libraries for List Virtualization
| Library               | Description                                                 |
| --------------------- | ----------------------------------------------------------- |
| **react-window**      | Lightweight, modern, and simple virtualization library      |
| **react-virtualized** | Full-featured, older library (grids, tables, lists)         |
| **react-virtuoso**    | Higher-level abstraction with smooth scrolling and grouping |
| **TanStack Virtual**  | Headless virtualization library for fine-grained control    |

### React Profiler
The **React Profiler** is a powerful tool that helps developers **measure performance** — specifically, to identify **which components re-render**, **how long rendering takes**, and **why** certain parts of your React app might be slow.

It’s available in the **React Developer Tools** browser extension.

The Profiler measures:
- Each **render** (initial and re-renders).
- The **duration** of each render.
- Which **components** were updated.
- The **cause** of each re-render (state, props, hooks, context).

You can **record** a profiling session, perform some interactions, then **stop recording** to analyze render performance.

React also provides a Profiler API component for measuring performance in code.
```jsx
import { Profiler } from "react";

function onRenderCallback(
  id, // Profiler id
  phase, // "mount" or "update"
  actualDuration, // time spent rendering
  baseDuration, // estimated duration if no memoization
  startTime,
  commitTime,
  interactions
) {
  console.log(`${id} took ${actualDuration}ms`);
}

export default function App() {
  return (
    <Profiler id="MainApp" onRender={onRenderCallback}>
      <MainApp />
    </Profiler>
  );
}
```

### Measuring Web Vitals in React (with `reportWebVitals.js`)

React apps created via **Create React App** include a `reportWebVitals.js` file by default.
```jsx
// src/reportWebVitals.js
import { getCLS, getFID, getLCP, getFCP, getTTFB } from 'web-vitals';

const reportWebVitals = (onPerfEntry) => {
  if (onPerfEntry && onPerfEntry instanceof Function) {
    getCLS(onPerfEntry);
    getFID(onPerfEntry);
    getLCP(onPerfEntry);
    getFCP(onPerfEntry);
    getTTFB(onPerfEntry);
  }
};

export default reportWebVitals;
```

Then in your index.js:
```jsx
import reportWebVitals from './reportWebVitals';

reportWebVitals(console.log);
```

This logs Web Vitals metrics in the console. You can replace console.log with a function that sends data to your analytics service.