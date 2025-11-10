---
sidebar_position: 3
---

# React Hooks

### What is Hook?
A **Hook** is a **special function** in React that lets you **use React features (like state, lifecycle, and context)** inside **functional components** —  
without writing class components.

Commonly Used Hooks
| Hook                      | Purpose                                                    |
| ------------------------- | ---------------------------------------------------------- |
| **`useState`**            | Add and manage local state inside a component.             |
| **`useEffect`**           | Run side effects (data fetching, subscriptions, timers).   |
| **`useContext`**          | Access values from React Context without prop drilling.    |
| **`useRef`**              | Persist values between renders or access DOM elements.     |
| **`useMemo`**             | Memoize expensive computations to optimize performance.    |
| **`useCallback`**         | Memoize functions to prevent unnecessary re-renders.       |
| **`useReducer`**          | Manage complex state logic with reducers (like Redux).     |
| **`useLayoutEffect`**     | Run effects synchronously after DOM mutations.             |
| **`useImperativeHandle`** | Expose controlled methods to parent components (via refs). |


### Rules of Hooks
React enforces two main rules to ensure Hooks behave predictably:
- Only call hooks at the top level: Do not call hooks inside loops, conditions, nested functions, or try/catch/finally blocks
- Only call hooks from React function components or custom hooks: Avoid using hooks in regular JavaScript functions

### `useState` Hook
It allows you to **add and manage local state** in functional components — something that previously was only possible in class components.

The useState Hook provides two things:
- A state variable to retain the data between renders.
- A state setter function to update the variable and trigger React to render the component again.
```jsx
const [state, setState] = useState(initialValue);
```
| Parameter          | Description                                                                 |
| ------------------ | --------------------------------------------------------------------------- |
| **`state`**        | The current state value.                                                    |
| **`setState`**     | Function used to update the state.                                          |
| **`initialValue`** | The initial value for the state (used only once when the component mounts). |



#### Important: State Updates Are Asynchronous
React batches state updates — meaning it groups multiple state changes together into a single re-render, instead of re-rendering after each update.

Example:
```jsx
setCount(count + 1);
setCount(count + 1);
```
You might expect count to increase by 2, but it increases only by 1, because both updates use the same stale value.

Use functional updates instead. A state variable’s value never changes within a render, even if its event handler’s code is asynchronous. So it is needed to to read the latest state before a re-render:
```jsx
setCount(prev => prev + 1);
setCount(prev => prev + 1); // Correctly increments by 2
```

### `useReducer` Hook
The **`useReducer`** Hook is an alternative to `useState` for managing **more complex state logic** in React components. It’s especially useful when state transitions depend on **previous state** or when managing **multiple related values**.

> 🧠 **In short:**  
> `useReducer` lets you manage component state using a **reducer function**, similar to how Redux works — but built directly into React.

```jsx
const [state, dispatch] = useReducer(reducer, initialState);
```
| Element            | Description                                                                    |
| ------------------ | ------------------------------------------------------------------------------ |
| **`reducer`**      | A pure function that determines how state changes.                             |
| **`state`**        | The current state value.                                                       |
| **`dispatch`**     | A function used to send (dispatch) an **action** that triggers a state update. |
| **`initialState`** | The default or starting state value.                                           |

A `reducer` function is where you will put your state logic. It takes two arguments, the current state and the action object, and it returns the next state.
A reducer is a pure function that:
- Takes the current state and an action.
- Returns a new state (never mutates the old one).

The object you pass to dispatch is called an `action`: It is a regular JavaScript object. You decide what to put in it, but generally it should contain the minimal information about what happened.

**Example:**
```jsx
import { useReducer } from "react";

function reducer(state, action) {
  switch (action.type) {
    case "increment":
      return { count: state.count + 1 };
    case "decrement":
      return { count: state.count - 1 };
    case "reset":
      return { count: 0 };
    default:
      return state;
  }
}

export default function Counter() {
  const [state, dispatch] = useReducer(reducer, { count: 0 });

  return (
    <div>
      <p>Count: {state.count}</p>
      <button onClick={() => dispatch({ type: "decrement" })}>-</button>
      <button onClick={() => dispatch({ type: "increment" })}>+</button>
      <button onClick={() => dispatch({ type: "reset" })}>Reset</button>
    </div>
  );
}
```

### `useEffect` Hook
`useEffect` runs code **after React renders the component**, keeping side effects separate from rendering logic.

Every time your component renders, React will update the screen and then run the code inside `useEffect` by default. In other words, `useEffect` “delays” a piece of code from running until that render is reflected on the screen.

The **`useEffect`** Hook lets you perform **side effects** in functional components — things like fetching data, manually updating the DOM, setting up subscriptions, or starting timers.

```jsx
useEffect(() => {
  // side effect logic
  return () => {
    // cleanup (optional)
  };
}, [dependencies]);
```
| Argument                        | Description                                                      |
| ------------------------------- | ---------------------------------------------------------------- |
| **Effect function**             | The code that runs after the component renders.                  |
| **Cleanup function (optional)** | Runs before the component unmounts or before the effect re-runs. |
| **Dependency array**            | Controls when the effect runs.                                   |

#### When useEffect Runs?
| Dependency Array               | Behavior                                         |
| ------------------------------ | ------------------------------------------------ |
| **No array**                   | Runs after every render.                         |
| **Empty array `[]`**           | Runs only once — after the first render (mount). |
| **With dependencies `[x, y]`** | Runs after mount + whenever `x` or `y` change.   |

### Difference between `useEffect` and `useLayoutEffect`?

- `useEffect` runs asynchronously, **after the component renders and the browser paints the screen**.
- `useLayoutEffect` runs right after React updates the DOM but before the browser paints. This means it blocks the paint until the effect finishes — useful for tasks that must run **synchronously** with DOM updates (like measurements or scroll positioning).

Use `useEffect` for most side effects — it’s non-blocking and safe for async work.

Use `useLayoutEffect` only when you need to measure or modify the DOM synchronously before the screen updates.

### `useEffectEvent` Hook
The `useEffectEvent` Hook helps you define **stable event-handler functions** inside functional components which always have access to the latest state or props, *without* forcing them to be dependencies of your `useEffect` calls. 

With a normal `useEffect`, if you reference a `prop` or `state` variable, you must include it in the dependency array — causing the effect to rerun when that value changes.

Example issue:
```jsx
useEffect(() => {
  connection.on('message', () => {
    // uses `currentUser` from props
    showAlert(currentUser.name);
  });
}, [roomId, currentUser]); // because currentUser changes → effect re-runs (disconnects & reconnects)
```
`useEffectEvent` gives you a handler that always “sees” the latest values but doesn’t force the parent effect to depend on them.

```jsx
import { useEffect, useEffectEvent, useState } from 'react';

function MyComponent({ roomId, theme }) {
  const [count, setCount] = useState(0);

  // Define a stable event function that uses latest `theme`
  const onConnected = useEffectEvent(() => {
    showNotification('Connected!', theme);
  });

  useEffect(() => {
    const connection = createConnection(roomId);
    connection.on('connected', () => {
      onConnected();  // uses latest theme, but effect only depends on roomId
    });
    connection.connect();
    return () => connection.disconnect();
  }, [roomId]);  // theme is *not* here, because onConnected handles that
}
```
- `onConnected` is the event handler.
- The effect depends only on `roomId`, so it doesn’t re-run when `theme` changes.
- Yet the handler always reflects the current `theme`.

### `useRef` Hook
The **`useRef`** Hook lets you create a **mutable reference object** that persists across component renders. 

It’s most often used to **access DOM elements directly**, or to **store values that don’t trigger re-renders** when changed.

When you want a component to “remember” some information, but you don’t want that information to trigger new renders, you can use a `ref`. Ref is mutable — you can modify and update current’s value outside of the rendering process.

```jsx
const ref = useRef(initialValue);
```
| Property           | Description                                |
| ------------------ | ------------------------------------------ |
| **`ref.current`**  | The mutable value stored by the reference. |
| **`initialValue`** | The starting value for the reference.      |

The object returned by `useRef()` is persistent — the same ref object exists for the entire lifetime of the component.

When to Use useRef
- Accessing DOM elements
- Storing values between renders - timers, intervals, previous props/state
- Avoiding re-renders
- Integration with third-party libraries

### Ref and Accessing a DOM Element
The most common use case for a `ref` is to access a DOM element. For example, this is handy if you want to focus an input programmatically.

When you pass a `ref` to a `ref` attribute in JSX, like `<div ref={myRef}>`, React will put the corresponding DOM element into `myRef.current`.

The `useRef` Hook returns an object with a single property called `current`. Initially, `myRef.current` will be `null`. When React creates a DOM node for this `<div>`, React will put a reference to this node into `myRef.current`. You can then access this DOM node from your event handlers and use the built-in browser APIs defined on it.

```jsx
import { useRef, useEffect } from "react";

function InputFocus() {
  const inputRef = useRef();

  useEffect(() => {
    inputRef.current.focus(); // Focus input on mount
  }, []);

  return <input ref={inputRef} placeholder="Focuses automatically" />;
}
```
- `inputRef` is attached to the `<input>` element.
- After the component renders, `inputRef.current` points to that DOM node.
- You can call native DOM methods (`focus`, `scrollIntoView`, etc.) directly.

### What is forwardRef() in React used for?
The **`forwardRef()`** function in React allows you to **pass a ref from a parent component down to a child component**, so the parent can directly access a **DOM element** or **child component instance** inside the child.
:::tip
In React 19, `forwardRef` is no longer necessary. Pass `ref` as a prop instead.
:::

By default, refs **cannot be passed through props**. When you attach a ref to a custom component, React assigns it to the **component instance**, not to any DOM node inside it.

```jsx
const Child = forwardRef((props, ref) => {
  return <input ref={ref} {...props} />;
});
```
| Parameter   | Description                        |
| ----------- | ---------------------------------- |
| **`props`** | Component props passed from parent |
| **`ref`**   | The forwarded ref from the parent  |


#### ❌ Without `forwardRef`
```jsx
function InputField() {
  return <input type="text" />;
}

function App() {
  const inputRef = useRef();

  // ❌ This ref points to <InputField>, not the <input> inside it
  return <InputField ref={inputRef} />;
}
```

Here, `inputRef.current` will be `undefined` — because `InputField` is a function component, not a DOM element.

#### ✅ Using forwardRef
To fix this, wrap the child component in `forwardRef()` and attach the `ref` to the desired DOM node.
```jsx
import { forwardRef, useRef } from "react";

const InputField = forwardRef((props, ref) => {
  return <input ref={ref} {...props} />;
});

function App() {
  const inputRef = useRef();

  const focusInput = () => {
    inputRef.current.focus(); // Works! Ref points to <input>
  };

  return (
    <div>
      <InputField ref={inputRef} placeholder="Type here..." />
      <button onClick={focusInput}>Focus Input</button>
    </div>
  );
}
```

### `useImperativeHandle` Hook
`useImperativeHandle()` is used to customize what the parent can access through the `ref`.

The **`useImperativeHandle`** Hook lets you **customize the instance value** that is exposed to parent components when using `ref`. It allows you to **control what the parent can access** from the child component.

> 🧠 **In short:**  
> `useImperativeHandle` defines **what methods or properties** the parent can call through a `ref` — instead of exposing the entire child DOM or internal logic.

```jsx
useImperativeHandle(ref, createHandle, [dependencies]);
```
| Parameter            | Description                                                                         |
| -------------------- | ----------------------------------------------------------------------------------- |
| **`ref`**            | The forwarded ref from the parent component (created via `forwardRef`).             |
| **`createHandle`**   | A function returning an object containing methods/properties the parent can access. |
| **`[dependencies]`** | Optional array of dependencies that re-create the handle when changed.              |

```jsx
import { forwardRef, useRef, useImperativeHandle, useState } from "react";

const InputField = forwardRef((props, ref) => {
  const inputRef = useRef();
  const [value, setValue] = useState("");

  // Define what the parent can access through the ref
  useImperativeHandle(ref, () => ({
    focus: () => inputRef.current.focus(),
    clear: () => setValue(""),
    getValue: () => value,
  }));

  return (
    <input
      ref={inputRef}
      value={value}
      onChange={(e) => setValue(e.target.value)}
      {...props}
    />
  );
});

function App() {
  const inputRef = useRef();

  return (
    <div>
      <InputField ref={inputRef} placeholder="Type something..." />
      <button onClick={() => inputRef.current.focus()}>Focus</button>
      <button onClick={() => inputRef.current.clear()}>Clear</button>
      <button onClick={() => alert(inputRef.current.getValue())}>Get Value</button>
    </div>
  );
}
```
- The parent (`App`) uses `ref` to communicate with the child.
- The child (`InputField`) controls what is exposed (`focus`, `clear`, `getValue`).
- The parent can call only those safe, explicitly defined methods.

Without `useImperativeHandle` parent gets a reference to the DOM element or component instance directly.

### `useMemo` Hook

The **`useMemo**` Hook lets you **memoize (cache)** the result of a **computed value** — so React **reuses** the previous result instead of recalculating it every render, unless its dependencies change.

```jsx
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```
| Parameter                       | Description                                                           |
| ------------------------------- | --------------------------------------------------------------------- |
| **Function (`() => ...`)**      | A function whose result you want to cache.                            |
| **Dependency array (`[a, b]`)** | React re-runs the function only if one of these dependencies changes. |
| **Returns**                     | The **cached result** of the computation.                             |

✅ Use useMemo when:
- You perform expensive computations (sorting, filtering, transformations).
- You want to stabilize references to prevent unnecessary child re-renders.
- You need derived data that depends on props or state.

⚠️ Avoid useMemo when:
- The computation is cheap (simple math or conditionals).
- You use it everywhere — overuse can hurt readability more than it helps.

### `useCallback` Hook
The **`useCallback`** Hook lets you **memoize (cache)** a function so that React doesn’t recreate it on every render — unless one of its dependencies changes.

```jsx
const memoizedCallback = useCallback(() => {
  // function logic here
}, [dependencies]);
```
| Parameter                       | Description                                          |
| ------------------------------- | ---------------------------------------------------- |
| **Function (`() => ...`)**      | The function you want to cache between renders.      |
| **Dependency array (`[deps]`)** | Function is recreated only when these values change. |
| **Returns**                     | A **stable (memoized)** function reference.          |

When to Use useCallback
- When passing callbacks to memoized children (`React.memo`, `useMemo`, etc.).
- When using callbacks as dependencies in `useEffect` or `useMemo`.
- When working with event listeners that must remain stable across renders.

### `React.memo` in React
`React.memo` makes a component render only when its **props change**.

The **`React.memo`** higher-order component (HOC) helps you **optimize functional components** by **preventing unnecessary re-renders**. It does this by **memoizing** the rendered output — React reuses the previous result if the component’s **props haven’t changed**.

```jsx
const MemoizedComponent = React.memo(Component, areEqual?);
```

| Parameter                 | Description                                                 |
| ------------------------- | ----------------------------------------------------------- |
| **`Component`**           | The functional component to memoize.                        |
| **`areEqual` (optional)** | Custom comparison function to determine if props are equal. |

```jsx
const Child = React.memo(({ count }) => {
  console.log("Child rendered");
  return <p>Count: {count}</p>;
});
```

You can provide a custom function to control when a component should re-render.
```jsx
function areEqual(prevProps, nextProps) {
  return prevProps.user.id === nextProps.user.id;
}

const UserCard = React.memo(({ user }) => {
  console.log("Rendered:", user.name);
  return <div>{user.name}</div>;
}, areEqual);
```

✅ Good for:
- Components that re-render often with the same props.
- Pure UI components (buttons, icons, list items).
- Components receiving stable props (via `useMemo` or `useCallback`).

⚠️ Avoid when:
- The component re-renders rarely (adds unnecessary overhead).
- Props are objects or functions that change references every render.
- Component is already cheap to render.

### `useId` Hook
The **`useId`** Hook generates a **unique and stable ID** that can be used for **accessibility attributes** (`id`, `htmlFor`, `aria-*`) or to **link form elements**. It ensures that IDs remain **consistent between server and client**, preventing mismatches during hydration in SSR (like Next.js).

```jsx
const id = useId();
```
| Variable | Description                                                |
| -------- | ---------------------------------------------------------- |
| **`id`** | A unique string generated by React, stable across renders. |

When Not to Use `useId`
| ❌ Don’t use for                        | Reason                                                                                |
| -------------------------------------- | ------------------------------------------------------------------------------------- |
| **Keys in lists (`key` prop)**         | Use stable item IDs instead; `useId` is for DOM attributes, not React reconciliation. |
| **Dynamic user data (e.g., from API)** | Use actual IDs from the data instead.                                                 |
| **Every small element**                | Only use where **unique accessibility IDs** are required.                             |

### `useOptimistic` Hook
The `useOptimistic` is a React Hook that lets you optimistically update the UI. Normally, when you perform an async action (e.g. sending a message, adding a todo) the UI waits for the server response before updating. With useOptimistic, you can:
- Immediately update the UI with a temporary optimistic value
- Then update again with the real result when the async call completes

```jsx
const [optimisticState, applyOptimistic] = useOptimistic(
  baseState,
  (currentState, optimisticValue) => {
    // return new optimistic state
  }
);
```
| Param         | Description                                                                                                                                           |
| ------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| `baseState`   | The “real” state value to show when no optimistic update is pending. ([React][1])                                                                     |
| `updateFn`    | A pure function `(currentState, optimisticValue) => newState` that defines how to apply the optimistic value. ([jser.dev][2])                         |
| Returns tuple | `[optimisticState, applyOptimistic]`: you read from `optimisticState` and call `applyOptimistic(value)` to trigger an optimistic update. ([React][1]) |

[1]: https://react.dev/reference/react/useOptimistic?utm_source=chatgpt.com "useOptimistic - React"
[2]: https://jser.dev/2024-03-20-how-does-useoptimisticwork-internally-in-react/?utm_source=chatgpt.com "How does useOptimistic() work internally in React? - JSer.dev"

**Simple Example**
```jsx
import { useOptimistic, useState, startTransition } from "react";

function LikeButton({ initialCount, sendLike }) {
  const [count, setCount] = useState(initialCount);

  const [optimisticCount, addOptimisticLike] = useOptimistic(
    count,
    (current, delta) => current + delta
  );

  function handleLike() {
    addOptimisticLike(1);               // immediately show +1
    startTransition(async () => {
      await sendLike();                 // real network action
      setCount(c => c + 1);             // commit real state update
    });
  }

  return <button onClick={handleLike}>👍 {optimisticCount}</button>;
}
```

### Custom hooks
A **Custom Hook** is a **reusable function** in React that uses one or more **built-in Hooks** (`useState`, `useEffect`, `useMemo`, etc.) to encapsulate and share logic between components.

| Rule                               | Description                                                              |
| ---------------------------------- | ------------------------------------------------------------------------ |
| 🪝 **Must start with "use"**       | React uses the “use” prefix to detect Hooks and enforce rules.           |
| 🔁 **Can use other hooks**         | Custom hooks can call `useState`, `useEffect`, etc.                      |
| 🔝 **Must be called at top level** | Just like built-in Hooks, they can’t be used inside loops or conditions. |

#### Example — Fetch Data Hook
```jsx
import { useState, useEffect } from "react";

function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    let ignore = false;
    setLoading(true);
    fetch(url)
      .then((res) => {
        if (!res.ok) throw new Error("Network error");
        return res.json();
      })
      .then((data) => !ignore && setData(data))
      .catch(setError)
      .finally(() => !ignore && setLoading(false));

    return () => { ignore = true; };
  }, [url]);

  return { data, loading, error };
}
```

### Common Examples of Custom Hooks
| Hook Name           | Description                                                           |
| ------------------- | --------------------------------------------------------------------- |
| `useLocalStorage`   | Sync and persist state to `localStorage`.                             |
| `useSessionStorage` | Sync and persist state to `sessionStorage`.                           |
| `useDebounce`       | Delay updates until a specified timeout passes after the last change. |
| `useThrottle`       | Limit how often a value or function can update.                       |
| `usePrevious`       | Keep track of the previous value of a variable.                       |
| `useToggle`         | Simplify boolean state management.                                    |
| `useOnClickOutside` | Detect clicks outside a referenced element.                           |
| `useMediaQuery`     | Track media query matches for responsive behavior.                    |
| `useWindowSize`     | Get and update current window width and height.                       |
| `useFetch`          | Handle API fetching logic with loading and error states.              |
| `useClipboard`      | Copy text to the clipboard programmatically.                          |
| `useEventListener`  | Add and clean up event listeners easily.                              |
| `useHover`          | Detect when an element is hovered.                                    |
| `useOnlineStatus`   | Track online/offline status of the user.                              |
| `useDarkMode`       | Manage light/dark theme preference.                                   |
| `useKeyPress`       | Detect when a specific keyboard key is pressed.                       |
| `useTimeout`        | Manage delayed function execution using `setTimeout`.                 |
| `useInterval`       | Run functions at a set interval using `setInterval`.                  |
| `useScrollPosition` | Track scroll position of a page or element.                           |

### `useSyncExternalStore` Hook
The **`useSyncExternalStore`** Hook provides a **standardized, efficient way to subscribe to external data sources** (e.g., global stores, browser APIs, or custom event emitters) while keeping React components in sync with those sources.

> 🧠 **In short:**  
> `useSyncExternalStore` is used to **safely read and subscribe to external stores** in React.

```jsx
const state = useSyncExternalStore(subscribe, getSnapshot, getServerSnapshot?);
```
| Parameter               | Description                                                                                                |
| ----------------------- | ---------------------------------------------------------------------------------------------------------- |
| **`subscribe`**         | A function that registers a callback to be called when the store changes. Returns an unsubscribe function. |
| **`getSnapshot`**       | Returns the current value of the store (the “snapshot”).                                                   |
| **`getServerSnapshot`** | (Optional) Used for SSR — provides the initial snapshot on the server.                                     |
| **Returns**             | The **current value** of the external store, kept in sync with React rendering.                            |

### `flushSync`
`flushSync` lets you **force React to update the DOM immediately**, instead of waiting for the normal (batched, async) update cycle.

```jsx
import { flushSync } from "react-dom";

flushSync(() => {
  // Perform state updates inside this callback
  setState(newValue);
});
```

React immediately applies the state update and commits it to the DOM synchronously — before continuing to the next JS operation.

**Example — Forcing Synchronous Update**
```jsx
import { flushSync } from "react-dom";
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  const handleClick = () => {
    flushSync(() => {
      setCount((c) => c + 1);
    });
    console.log("DOM updated, new count:", document.getElementById("count").textContent);
  };

  return (
    <p id="count" onClick={handleClick}>
      Count: {count}
    </p>
  );
}
```
- Normally, React batches updates for performance — DOM isn’t updated immediately.
- By wrapping setCount in flushSync, React flushes the DOM update synchronously.
- The next line (`console.log`) sees the updated DOM right away.

:::warning
Use `flushSync` only when you must see the DOM immediately after a state change (Measure DOM after update, Transition start, etc.).
:::
