---
sidebar_position: 5
---

# React Context

### What is React Context
**React Context** is a built-in mechanism for **sharing data across the component tree** without having to pass props manually through every level (“prop drilling”).

Whenever the context provider value changes, all descendants consuming the context (`useContext(...)`) will be re-rendered.

### Creating Context
1. Creating Context - First, you need to create the context. You’ll need to export it from a file so that your components can use it. The only argument to createContext is the default value. You could pass any kind of value (even an object). 
```jsx
const MyContext = React.createContext(defaultValue);
```
2. Providing Context
```jsx
<MyContext.Provider value={data}>
  <Child />
</MyContext.Provider>
```
3. Consuming Context
```jsx
const value = useContext(MyContext);
```

Example — Theme Context
```jsx
import { createContext, useContext, useState } from "react";

const ThemeContext = createContext();

function ThemeProvider({ children }) {
  const [theme, setTheme] = useState("light");
  const toggleTheme = () => setTheme((t) => (t === "light" ? "dark" : "light"));

  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
}

function ThemedButton() {
  const { theme, toggleTheme } = useContext(ThemeContext);
  return (
    <button
      onClick={toggleTheme}
      style={{
        background: theme === "light" ? "#eee" : "#333",
        color: theme === "light" ? "#000" : "#fff",
      }}
    >
      Toggle Theme
    </button>
  );
}

export default function App() {
  return (
    <ThemeProvider>
      <ThemedButton />
    </ThemeProvider>
  );
}
```

### Multiple Contexts
You can nest multiple Providers to manage different global states independently.
```jsx
<ThemeContext.Provider value={theme}>
  <AuthContext.Provider value={user}>
    <App />
  </AuthContext.Provider>
</ThemeContext.Provider>
```

### What problem does React Context solve?
React Context solves the problem of **prop drilling** — the process of passing data through multiple levels of components that **don’t need it directly**, just to deliver it to a deeply nested child.

### How do you update Context values?
Usually by passing setter functions or dispatchers inside the Provider’s `value`.

### What happens when the Provider’s value changes?
All consuming components re-render — even if they don’t use all parts of the `value`.

### How do you avoid unnecessary re-renders caused by Context updates?
Memoize value with useMemo, or split large Contexts into smaller ones.

### When should you not use Context?
For frequently changing data (e.g., input text, animations) — use local state instead.

### How does Context compare to state management libraries like Redux or Zustand?
Context is simpler and local; Redux/Zustand handle larger, more complex state and middleware.

### How can you structure large apps that use multiple Contexts?
Group related contexts (AuthContext, ThemeContext, SettingsContext), and compose them in a `RootProvider`.
