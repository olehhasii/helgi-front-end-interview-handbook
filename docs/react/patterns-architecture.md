---
sidebar_position: 6
---

# React and Components Architecture/Patterns

### Component Design in React
1. Key Principles of Good Component Design
| Principle | Description |
|------------|--------------|
| **Single Responsibility** | Each component should do one thing well — e.g., display a list, handle input, show data. |
| **Reusability** | Design components so they can be reused with different data or contexts. |
| **Composability** | Build complex UIs by composing smaller components together. |
| **Isolation** | Components should be independent — changes in one shouldn’t break others. |
| **Readability** | Component names, props, and structure should clearly express their purpose. |

2. Types of Components
| Type | Description | Example |
|------|--------------|----------|
| **Presentational (UI)** | Focus on how things look (stateless). | Buttons, Cards, Lists |
| **Container (Logic)** | Handle data fetching, state, and logic (stateful). | ProductListContainer, UserProvider |
| **Layout** | Define structural composition. | Header, Sidebar, PageLayout |
| **Higher-Order / Hook-based** | Reuse behavior or logic. | useFetch, withAuthProtection |
| **Atomic / Utility** | Low-level building blocks for consistent design. | Icon, InputField, Spinner |

3. Breaking UI Into Components
Start by identifying **repeating patterns** and **logical boundaries**.

Example — eCommerce product card:

```jsx
<ProductList>
  <ProductCard>
    <ProductImage />
    <ProductDetails />
    <AddToCartButton />
  </ProductCard>
</ProductList>
```
Each part can be implemented, styled, and tested independently.

4. Composition Over Inheritance
React encourages composition — passing components or elements as `children` or `props`.
```jsx
function Card({ title, children }) {
  return (
    <div className="card">
      <h3>{title}</h3>
      <div className="content">{children}</div>
    </div>
  );
}

// Usage
<Card title="Welcome">
  <p>This is a card with custom content!</p>
</Card>
```

### Higher-order components (HOC)
A Higher-Order Component is a **function that takes a component as input and returns a new component** with added functionality.

```jsx
const EnhancedComponent = higherOrderComponent(WrappedComponent);
```
- `WrappedComponent` → The original component (input).
- `EnhancedComponent` → The new component returned with extra behavior (output).

HOCs are used to:
- Share common functionality between components
- Abstract and reuse component logic
- Enhance components with additional props or state
- Authentication: Wrapping components to check if a user is authenticated before rendering.
- Logging: Adding logging functionality to components.
- Theming: Injecting theme-related props into components.

Key Rules of HOCs
| Rule                                      | Description                                             |
| ----------------------------------------- | ------------------------------------------------------- |
| **Don’t mutate the original component**   | Always return a new one instead of modifying input.     |
| **Pass props through**                    | Always forward all received props using `{...props}`.   |
| **Wrap display names**                    | Give meaningful names to HOCs for debugging.            |
| **Avoid copying static methods manually** | Use libraries like `hoist-non-react-statics` if needed. |

#### Example — Logging Props
```jsx
function withLogger(WrappedComponent) {
  return function EnhancedComponent(props) {
    console.log("Props:", props);
    return <WrappedComponent {...props} />;
  };
}

// Usage
function Hello({ name }) {
  return <h1>Hello, {name}</h1>;
}

const HelloWithLogger = withLogger(Hello);
```

#### Example — Adding Authentication Check
```jsx
function withAuth(WrappedComponent) {
  return function AuthenticatedComponent(props) {
    const isLoggedIn = Boolean(localStorage.getItem("token"));
    if (!isLoggedIn) {
      return <p>Please log in first.</p>;
    }
    return <WrappedComponent {...props} />;
  };
}

// Usage
const DashboardWithAuth = withAuth(Dashboard);
```

### Render Props
Render props is a pattern in React for sharing code between components using a prop whose value is a function.

A **Render Prop** is a **technique for sharing logic between components** by using a **function as a prop**. Instead of hardcoding what a component renders, you give control back to the consumer — it decides *how* to render the data or behavior provided by that component.

```jsx
<DataProvider render={(data) => <Display data={data} />} />
```

**Why Use Render Props?**
| Benefit                   | Description                                               |
| ------------------------- | --------------------------------------------------------- |
| **Logic Reuse**           | Share logic across components without inheritance or HOCs |
| **Custom Rendering**      | Components can decide how data should appear              |
| **Composition Friendly**  | Can be combined with any other React patterns             |
| **Predictable Data Flow** | Keeps control in the parent component                     |


#### Example — Mouse Tracker
```jsx
function MouseTracker({ render }) {
  const [position, setPosition] = useState({ x: 0, y: 0 });

  useEffect(() => {
    const handleMouseMove = (event) => {
      setPosition({ x: event.clientX, y: event.clientY });
    };
    window.addEventListener('mousemove', handleMouseMove);

    return () => window.removeEventListener('mousemove', handleMouseMove);
  }, []);

  return render(position);
}

function App() {
  return (
    <MouseTracker
      render={(position) => (
        <p>
          Mouse: {position.x}, {position.y}
        </p>
      )}
    />
  );
}
```

### Container/Presentational Pattern
The **Container/Presentational pattern** is a classic React design pattern used to **separate logic from UI**.  

It divides components into two main categories:
1. **Presentational Components**: Components that care about how data is shown to the user.
2. **Container Components**: Components that care about what data is shown to the user. 

#### Presentational Component
A presentational component receives its data through `props`. Its primary function is to simply display the data it receives the way we want them to, including styles, without modifying that data.

Presentational components are usually stateless: they do not contain their own React state, unless they need a state for UI purposes. The data they receive, is not altered by the presentational components themselves.

```jsx
function UserList({ users }) {
  return (
    <ul>
      {users.map((u) => (
        <li key={u.id}>{u.name}</li>
      ))}
    </ul>
  );
}
```
- Only receives users via props.
- Doesn’t know where the data comes from.
- Focused purely on layout.

#### Container Components
The primary function of container components is to **pass data** to presentational components, which they contain. Container components themselves usually don’t render any other components besides the presentational components that care about their data. Since they don’t render anything themselves, they usually do not contain any styling either.

```jsx
function UserListContainer() {
  const [users, setUsers] = useState([]);

  useEffect(() => {
    fetch("/api/users")
      .then((res) => res.json())
      .then(setUsers);
  }, []);

  return <UserList users={users} />;
}
```
- Handles fetching and state management.
- Passes data down to the presentation layer.

#### Hooks
In many cases, the Container/Presentational pattern can be replaced with React Hooks. The introduction of Hooks made it easy for developers to add statefulness without needing a container component to provide that state.

- Custom Hooks now often replace container components.
- Logic moves to hooks, while UI stays in the presentation component.

```jsx
// Hook (logic)
function useUsers() {
  const [users, setUsers] = useState([]);
  useEffect(() => {
    fetch("/api/users")
      .then((res) => res.json())
      .then(setUsers);
  }, []);
  return users;
}

// Presentational
function UserList({ users }) {
  return <ul>{users.map((u) => <li key={u.id}>{u.name}</li>)}</ul>;
}

// Container (simplified)
function UserListContainer() {
  const users = useUsers();
  return <UserList users={users} />;
}
```

### Compound Pattern
In our application, we often have components that belong to each other. They’re dependent on each other through the shared state, and share logic together. You often see this with components like select, dropdown components, or menu items. The compound component pattern allows you to create components that all work together to perform a task.

The **Compound Components pattern** is a design approach that lets multiple components **work together as a cohesive unit** — sharing implicit state and behavior through composition, rather than through deeply nested props.

Normally, you might pass many props to control a complex component:
```jsx
<Tabs
  activeTab={activeTab}
  onChange={setActiveTab}
  tabs={[
    { label: "Home", content: <Home /> },
    { label: "Profile", content: <Profile /> },
  ]}
/>
```
⚠️ This becomes cumbersome and rigid as your UI grows.

Compound components let developers express this more declaratively:
```jsx
<Tabs>
  <Tabs.List>
    <Tabs.Tab>Home</Tabs.Tab>
    <Tabs.Tab>Profile</Tabs.Tab>
  </Tabs.List>
  <Tabs.Panels>
    <Tabs.Panel>Home content</Tabs.Panel>
    <Tabs.Panel>Profile content</Tabs.Panel>
  </Tabs.Panels>
</Tabs>
```

#### Example
Parent Component (`Toggle`)
```jsx
const ToggleContext = React.createContext();

function Toggle({ children }) {
  const [on, setOn] = React.useState(false);
  const toggle = () => setOn((o) => !o);

  return (
    <ToggleContext.Provider value={{ on, toggle }}>
      {children}
    </ToggleContext.Provider>
  );
}
```
Child Components
```jsx
function ToggleOn({ children }) {
  const { on } = React.useContext(ToggleContext);
  return on ? children : null;
}

function ToggleOff({ children }) {
  const { on } = React.useContext(ToggleContext);
  return on ? null : children;
}

function ToggleButton() {
  const { on, toggle } = React.useContext(ToggleContext);
  return (
    <button onClick={toggle}>
      {on ? "Turn Off 🔴" : "Turn On 🟢"}
    </button>
  );
}
```
Usage
```jsx
function App() {
  return (
    <Toggle>
      <ToggleOn>The button is ON</ToggleOn>
      <ToggleOff>The button is OFF</ToggleOff>
      <ToggleButton />
    </Toggle>
  );
}
```

#### `React.Children.map()` + `cloneElement`
```jsx
function Toggle({ children }) {
  const [on, setOn] = React.useState(false);
  return React.Children.map(children, (child) =>
    React.cloneElement(child, { on, toggle: () => setOn(!on) })
  );
}
```
Works without context but less flexible for deeply nested components.

### Composition pattern
The **Composition Pattern** is one of the **core principles of React** — it’s how you build complex UIs by **combining smaller, reusable components** together.  

Rather than relying on inheritance, React promotes **composition** — assembling components like building blocks.

React avoids classical inheritance (like in OOP) and encourages **composition** — where behavior and UI are shared by **nesting components** or **passing them as children**.

Every React component automatically receives a `children` prop, which represents anything nested inside it.

```jsx
function Card({ children }) {
  return <div className="card">{children}</div>;
}

// Usage
<Card>
  <h3>Title</h3>
  <p>Some description here.</p>
</Card>
```
This is component composition: the `Card` doesn’t know what’s inside — it just provides structure and styling.

#### Example — Multiple “Slots” with Custom Props
You can also pass multiple components or elements as props to define custom areas (“slots”).
```jsx
function Layout({ header, sidebar, content }) {
  return (
    <div>
      <header>{header}</header>
      <aside>{sidebar}</aside>
      <main>{content}</main>
    </div>
  );
}

// Usage
<Layout
  header={<h1>My App</h1>}
  sidebar={<nav>Menu</nav>}
  content={<p>Welcome to the dashboard.</p>}
/>
```

### Flux pattern
The **Flux pattern** is an **application architecture** introduced by Facebook for managing **unidirectional data flow** in React applications.  

**Flux** enforces a **one-way data flow** — data moves in a single direction through four main parts: **Action → Dispatcher → Store → View (React Component)**.

1. **Actions** — Describe *what happened* (e.g., “USER_LOGGED_IN”).  
2. **Dispatcher** — The central hub that delivers actions to the correct stores.  
3. **Store** — Holds the state and business logic for a specific domain.  
4. **View (React Component)** — Reads data from stores and triggers new actions.

#### Action
An **action** is a simple object or function describing an event or user interaction.

```jsx
const addTodoAction = {
  type: "ADD_TODO",
  payload: { text: "Learn Flux" },
};
```

#### Dispatcher
The dispatcher is a central event hub that broadcasts actions to stores.
```jsx
import { Dispatcher } from "flux";

const AppDispatcher = new Dispatcher();
AppDispatcher.dispatch(addTodoAction);
```

#### Store
A store holds the app’s state and updates it in response to actions.
```jsx
import { EventEmitter } from "events";
import AppDispatcher from "./AppDispatcher";

class TodoStore extends EventEmitter {
  constructor() {
    super();
    this.todos = [];

    AppDispatcher.register((action) => {
      if (action.type === "ADD_TODO") {
        this.todos.push(action.payload.text);
        this.emit("change"); // Notify views
      }
    });
  }

  getTodos() {
    return this.todos;
  }
}

export default new TodoStore();
```

#### View (React Component)
The view listens for store updates and renders UI accordingly.
```jsx
import React, { useEffect, useState } from "react";
import TodoStore from "./TodoStore";
import AppDispatcher from "./AppDispatcher";

function TodoApp() {
  const [todos, setTodos] = useState(TodoStore.getTodos());

  useEffect(() => {
    const handleChange = () => setTodos([...TodoStore.getTodos()]);
    TodoStore.on("change", handleChange);
    return () => TodoStore.removeListener("change", handleChange);
  }, []);

  const addTodo = () =>
    AppDispatcher.dispatch({
      type: "ADD_TODO",
      payload: { text: "New Task" },
    });

  return (
    <>
      <button onClick={addTodo}>Add Todo</button>
      <ul>{todos.map((t, i) => <li key={i}>{t}</li>)}</ul>
    </>
  );
}
```