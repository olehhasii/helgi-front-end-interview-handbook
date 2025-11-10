---
sidebar_position: 2
---

# State and Props

### What Is State in React?
**State** is a built-in object in React components that holds data or information about the component. It is managed within the component and can change over time, usually in response to user actions or network responses. State can be viewed as the "memory" of a component instance. Each component instance's state is isolated and cannot be directly modified from external.

When state changes, **React automatically re-renders** the component to reflect the new data in the UI.

You manage state using the **`useState()`** hook.

State Is Immutable. You should not mutate state directly — always use the setter function.

State is local to a component instance on the screen. In other words, if you render the same component twice, each copy will have completely isolated state! Changing one of them will not affect the other.

### What Are Props in React?
**Props** (short for **properties**) are how React components **receive data from their parent components**. 

They are used to pass data and event handlers down the component tree. Props are **immutable**, meaning they cannot be changed by the child component.

Props allow you to:
- Give components **different data** each time you use them.
- Keep logic centralized while customizing content.

### What is the difference between state and props in React?
- State is managed within the component, while props are passed from a parent component
- State can change over time, while props are immutable
- State is used for data that changes within a component, while props are used to pass data and event handlers to child components

| Feature         | **Props**                      | **State**                          |
| --------------- | ------------------------------ | ---------------------------------- |
| Definition      | Data passed from parent        | Internal data of a component       |
| Mutability      | Immutable (read-only)          | Mutable (via setter)               |
| Ownership       | Controlled by parent           | Controlled by the component itself |
| Usage           | Configuration and data passing | Managing dynamic data              |
| Access          | `props.name`                   | `[value, setValue] = useState()`   |
| Direction       | Parent → Child                 | Internal                           |
| Update Triggers | Parent re-render               | Component re-render                |

### What is the difference between controlled and uncontrolled components?

#### Form Elements
A **controlled component** is a form element whose **value is controlled by React state**. React acts as the *single source of truth* for the input’s data.

An uncontrolled component stores its data in the DOM itself, not in React state. You access the value using `refs` instead of `useState`.
```jsx
import { useRef } from "react";

function UncontrolledInput() {
  const inputRef = useRef();

  const handleSubmit = () => {
    alert(`Entered name: ${inputRef.current.value}`);
  };

  return (
    <div>
      <input type="text" ref={inputRef} /> {/* DOM manages value */}
      <button onClick={handleSubmit}>Submit</button>
    </div>
  );
}
```
> 🧠 **In short:**  
> - **Controlled components** → React is in charge of the form data.  
> - **Uncontrolled components** → The DOM keeps track of the form data.

#### Components
A **controlled component** is one where its behavior is fully determined by its `props` rather than its own local state. This allows the parent component to have full control over how the component behaves.

An **uncontrolled component** manages its own state internally, making it independent of the parent component. The parent cannot directly modify the state of an uncontrolled component.

### What happens when the state is updated in React?

#### 1. State Update Request
When you call a state updater function like `setCount(newValue)` or `setCount(prev => prev + 1)`:

- React **schedules** a state update.
- The component is **marked as “dirty”** (needs to re-render).
- The actual state doesn’t change immediately — React batches updates for performance.

#### 2. Scheduling and Batching
React batches multiple updates within the same event loop tick to avoid unnecessary re-renders.

#### 3. Re-rendering the Component
1. React re-runs the component function (for function components).
2. The component executes again, calling all hooks in the same order.
3. The new state value is used during this render.

### How to force React to reset component’s state?
React resets a component’s state **only when it unmounts and mounts again**. You can force this behavior by changing the component’s **`key` prop** or **render components in different positions** it.

