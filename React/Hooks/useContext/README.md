# useContext Hook

`useContext` allows any component in your component tree to "tune in" and access data directly from a provider, bypassing the need to pass props down manually through every level (prop drilling).

## 1. How It Works: The 3-Step Setup

To use Context effectively, you need three distinct parts:

1.  **The Context Object**: The "radio station" channel.
2.  **The Provider**: The "broadcast tower" that wraps a section of your app and holds the data.
3.  **The Consumer (`useContext`)**: The receiver in the child component.

### Example: Theme Toggling

```javascript
import { createContext, useContext, useState } from 'react';

// 1. Create the Context ("The Radio Station")
const ThemeContext = createContext(null);

export default function App() {
  const [theme, setTheme] = useState('light');

  return (
    // 2. Provide the Value ("The Broadcast Tower")
    <ThemeContext.Provider value={{ theme, setTheme }}>
      <Page />
    </ThemeContext.Provider>
  );
}

function Page() {
  return <Button />; // No props passed here!
}

function Button() {
  // 3. Consume the Value ("The Receiver")
  const { theme, setTheme } = useContext(ThemeContext);
  
  return (
    <button onClick={() => setTheme(theme === 'light' ? 'dark' : 'light')}>
      Current Theme: {theme}
    </button>
  );
}
```

## 2. Under the Hood: Dependency Tracking

When you wrap a component tree in a Provider, React attaches a special property to the internal nodes.

- When a component calls `useContext(MyContext)`, React marks it as a **subscriber** to that context.
- unlike `useState` which acts locally, a change in the `Context.Provider`'s value triggers a re-render for **every single component** that calls `useContext` for that provider, regardless of where they are in the tree.

## 3. The Major Drawback: Unnecessary Re-renders


### The Problem: Direct Nesting
When you define components directly inside a Provider (without using the `{children}` prop), those components are technically being "called" and re-created every time the Provider function runs/re-renders.

```javascript
function ThemeProvider() {
  const [theme, setTheme] = useState('light');
  const value = { theme, setTheme }; 

  return (
    <ThemeContext.Provider value={value}>
      {/* This will re-render every time theme changes, even if it doesn't use context! */}
      <Sidebar />
    </ThemeContext.Provider>
  );
}
```

### The Solution: Using `children`
When you accept `children` as a prop, you represent the "slots" that were filled by the parent component. When `ThemeProvider` re-renders, React knows looking at the `children` prop: *"Ah, these are the exact same component instances as before. I don't need to re-render them unless their own props changed."*

```javascript
function ThemeProvider({ children }) {
  const [theme, setTheme] = useState('light');
  const value = { theme, setTheme };

  return (
    <ThemeContext.Provider value={value}>
      {children} {/* Stable reference! won't re-render unless it consumes Context */}
    </ThemeContext.Provider>
  );
}

// Usage
<ThemeProvider>
  <Sidebar />
</ThemeProvider>
```
## 4. Optimization Strategies

**The Problem**: If your Context value is a composite object (e.g., `{ user, theme }`) and you update only one part (e.g., `user`), *every* component consuming that context will re-render—even if it only uses `theme`. This happens because React detects a new object reference in the `value` prop.

### A. Split Your Contexts (Best Practice)
Don't dump everything into a single `GlobalContext`. Separate concerns by domain.
- `UserContext` for authentication.
- `ThemeContext` for UI styles.
*Result*: If the user logs out, components only listening to the Theme won't needlessly re-render.

### B. Stabilize Values with `useMemo`
Create a specialized provider component to isolate state logic and ensure object references remain stable unless data actually changes.

```javascript
export function ThemeProvider({ children }) {
  const [theme, setTheme] = useState('light');
  
  // Memoize the value object so it doesn't get recreated on every render
  const value = useMemo(() => ({ theme, setTheme }), [theme]);

  return (
    <ThemeContext.Provider value={value}>
      {children}
    </ThemeContext.Provider>
  );
}
```

### C. Use `React.memo`
If a component sits between the provider and consumer but doesn't use the context itself, wrap it in `React.memo` to prevent it from re-rendering when the parent updates.

If you are already using the children prop pattern, you usually do not need React.memo for the components passed as children.

## 6. Common Use Cases

- **Theming**: Toggling Dark/Light modes.
- **User Authentication**: Storing user profiles and auth tokens globally.
- **Localization**: Switching languages (i18n).
- **Feature Flags**: Toggling specific UI features based on permissions.
