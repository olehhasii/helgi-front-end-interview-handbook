---
sidebar_position: 4
---

# Events in React

### How React handles events
React uses its own **synthetic event system** — a lightweight wrapper around the browser’s native events. This system ensures **cross-browser consistency**, **performance optimizations**, and **unified event behavior** across platforms.

Instead of attaching event listeners directly to every DOM element, React uses **event delegation** — it attaches a **single listener** to the **root DOM node** (e.g., `document` or React root container).

### 🔧 How it works
1. You declare event handlers in JSX:
   ```jsx
   <button onClick={handleClick}>Click Me</button>
   ```
2. React internally registers this event at the root level (not on the `button` directly).
3. When you click the button:
    - The browser triggers the native event.
    - React catches it at the root via delegation.
    - React then dispatches the synthetic event to your component’s handler.

React wraps native events inside a `SyntheticEvent` object.
```jsx
function handleClick(e) {
  console.log(e.type); // "click"
  console.log(e.nativeEvent); // Original browser event
}
```
**Properties of SyntheticEvent**
| Property           | Description                               |
| ------------------ | ----------------------------------------- |
| `type`             | The event type (`click`, `keydown`, etc.) |
| `target`           | The element that triggered the event      |
| `currentTarget`    | The element the listener is attached to   |
| `nativeEvent`      | The original browser event                |
| `bubbles`          | Whether the event bubbles                 |
| `defaultPrevented` | True if `preventDefault()` was called     |

### Event Phases: Bubbling and Capturing
In React (and the DOM), every event goes through **three main phases**: 1️⃣ **Capturing Phase** → 2️⃣ **Target Phase** → 3️⃣ **Bubbling Phase**

| Phase | Direction | Description |
|--------|------------|--------------|
| **Capturing** | Root → Target | Event travels **down** from the root to the clicked element. |
| **Target** | — | Event reaches the **target element** (where it originated). |
| **Bubbling** | Target → Root | Event travels **up** from the target back through its ancestors. |

#### Bubbling Phase (Default in React)
By default, React event handlers fire **during the bubbling phase**.
```jsx
function Example() {
  return (
    <div onClick={() => console.log("DIV clicked")}>
      <button onClick={() => console.log("BUTTON clicked")}>
        Click me
      </button>
    </div>
  );
}
```
Output order:
```
BUTTON clicked
DIV clicked
```

#### Capturing Phase in React
To handle events before they reach the target element, use the capture phase handler — by adding the suffix `Capture`.
```jsx
function Example() {
  return (
    <div onClickCapture={() => console.log("DIV captured click")}>
      <button onClick={() => console.log("BUTTON clicked")}>
        Click me
      </button>
    </div>
  );
}
```
Output order:
```
DIV captured click
BUTTON clicked
```

### Stopping event propagation
One way to intercept an event in React is by using `event.stopPropagation()`, which prevents the event from bubbling up to parent components. By default, events in React follow the bubbling phase, meaning they propagate from the target element up through its ancestors.

### Preventing default behavior
Another form of event interception is preventing default browser actions using `event.preventDefault()`. Certain HTML elements, like `<form>`, `<a>`, and `<input>`, have built-in behaviors (e.g., form submission, link navigation). If you want to handle these interactions with custom logic in React, you must prevent their default behavior.

### Mouse events
| Event | Triggered When | Typical Use |
|--------|----------------|--------------|
| **`onClick`** | Mouse button is clicked and released on the element | Buttons, links, clickable areas |
| **`onDoubleClick`** | Mouse is double-clicked | Double-tap actions, selections |
| **`onMouseDown`** | Mouse button is pressed down | Start of drag or drawing |
| **`onMouseUp`** | Mouse button is released | End of drag or click sequence |
| **`onMouseMove`** | Mouse pointer moves over an element | Tracking cursor position, drawing |
| **`onMouseEnter`** | Pointer enters an element (no bubbling) | Hover effects |
| **`onMouseLeave`** | Pointer leaves an element (no bubbling) | Hover exit, tooltips |
| **`onMouseOver`** | Pointer moves over an element (bubbles) | Similar to `onMouseEnter` but bubbles |
| **`onMouseOut`** | Pointer leaves an element (bubbles) | Opposite of `onMouseOver` |
| **`onContextMenu`** | Right mouse button is clicked | Custom right-click menus |

### Input Events
| Event | Triggered When | Typical Use |
|--------|----------------|--------------|
| **`onChange`** | Input value changes (fires on each keystroke in React) | Track form input in real time |
| **`onInput`** | Input value changes (similar to `onChange`, less commonly used) | Low-level input tracking |
| **`onFocus`** | Input element gains focus | Highlight or validate input field |
| **`onBlur`** | Input element loses focus | Validation or cleanup on blur |
| **`onKeyDown`** | Key is pressed down | Handle hotkeys or special key detection |
| **`onKeyUp`** | Key is released | Detect input completion |
| **`onSelect`** | User selects text in an input/textarea | Handle text selection events |
| **`onInvalid`** | Form validation fails (HTML5 validation) | Custom validation messages |

#### onChange vs onInput
| Feature            | `onChange`                                        | `onInput`                                                         |
| ------------------ | ------------------------------------------------- | ----------------------------------------------------------------- |
| **React Behavior** | Fires on **every keystroke** (unlike native HTML) | Fires on every input change (native behavior)                     |
| **Timing**         | Synchronous with value updates                    | Slightly lower-level                                              |
| **Use case**       | Recommended for controlled inputs                 | Use when fine-grained control is needed (e.g. composition events) |

#### Handling Checkboxes and Selects
Checkbox:
```jsx
function Checkbox() {
  const [checked, setChecked] = useState(false);

  return (
    <label>
      <input
        type="checkbox"
        checked={checked}
        onChange={(e) => setChecked(e.target.checked)}
      />
      Accept Terms
    </label>
  );
}
```
Select:
```jsx
function SelectExample() {
  const [fruit, setFruit] = useState("apple");

  return (
    <select value={fruit} onChange={(e) => setFruit(e.target.value)}>
      <option value="apple">Apple</option>
      <option value="banana">Banana</option>
      <option value="orange">Orange</option>
    </select>
  );
}
```

### Form events
| Event | Triggered When | Typical Use |
|--------|----------------|--------------|
| **`onSubmit`** | User submits the form (e.g., presses Enter or clicks submit) | Handle form data and prevent reload |
| **`onReset`** | Form is reset (via reset button or JS) | Restore default values |

### Keyboard events
| Event | Triggered When | Typical Use |
|--------|----------------|--------------|
| **`onKeyDown`** | Key is pressed down | Detect key press start, shortcuts |
| **`onKeyUp`** | Key is released | Trigger actions after typing or key release |
| **`onKeyPress`** *(deprecated)* | Key that produces a character is pressed | Legacy, avoid in favor of `onKeyDown` |

:::tip
`e.key` gives the actual key pressed (e.g., "a", "Enter", "Shift").
`e.code` - Physical key location on keyboard ("KeyA", "ArrowUp")
`e.keyCode` - Numeric key code (legacy) (`13`, `56`)
:::