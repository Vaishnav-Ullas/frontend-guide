# React Hooks

React Hooks are functions that allow you to use state, lifecycle, and other React features within functional components, without needing to write class components. They were introduced in **React 16.8** to promote code reusability, simplify component logic, and improve performance.

## 1. Why Hooks?
Hooks solved three major problems in React:

1.  **Complexity**: You no longer need to understand `this` or bind event handlers.
2.  **Reusability**: You can extract logic into **Custom Hooks** and share them across components.
3.  **Organization**: Related code (like fetching data and cleaning up a timer) can stay together instead of being split across different lifecycle methods.

## 2. Rules of Hooks
Two rules must be followed when using hooks:

1.  **Top-Level Calls**: Hooks should only be called at the top level of a function component, **not** inside loops, conditions, or nested functions.
2.  **React Function Calls**: Hooks must only be called from **React function components** or **custom hooks**, not regular JavaScript functions.

## 3. Built-in React Hooks
Hooks let you use different React features from your components. You can either use the built-in Hooks or combine them to build your own (Custom Hooks).

### State Hooks
State lets a component “remember” information like user input.
*   `useState`: Declares a state variable that you can update directly.
*   `useReducer`: Declares a state variable with the update logic inside a reducer function.

```javascript
function ImageGallery() {
  const [index, setIndex] = useState(0);
}
```

### Context Hooks
Context lets a component receive information from distant parents without passing it as props.
*   `useContext`: Reads and subscribes to a context.

```javascript
function Button() {
  const theme = useContext(ThemeContext);

}
```

### Ref Hooks
Refs let a component hold some information that isn’t used for rendering, like a DOM node or a timeout ID. Unlike state, updating a ref **does not re-render** your component. They are an “escape hatch” for working with non-React systems.
*   `useRef`: Declares a ref. Usually used to hold a DOM node.
*   `useImperativeHandle`: Lets you customize the ref exposed by your component (rarely used).

```javascript
function Form() {
  const inputRef = useRef(null);

}
```

### Effect Hooks
Effects let a component connect to and synchronize with external systems (network, browser DOM, animations).
*   `useEffect`: Connects a component to an external system.

```javascript
function ChatRoom({ roomId }) {
  useEffect(() => {
    const connection = createConnection(roomId);
    connection.connect();
    return () => connection.disconnect();
  }, [roomId]);
}
```

**Rare Variations:**
*   `useLayoutEffect`: Fires before the browser repaints the screen (measure layout).
*   `useInsertionEffect`: Fires before React makes changes to the DOM (for dynamic CSS libraries).

### Performance Hooks
A common way to optimize re-rendering performance is to skip unnecessary work.
*   `useMemo`: Caches the result of an expensive calculation.
*   `useCallback`: Caches a function definition before passing it to an optimized component.

```javascript
function TodoList({ todos, tab, theme }) {
  const visibleTodos = useMemo(() => filterTodos(todos, tab), [todos, tab]);
}
```

**Prioritizing Rendering:**
*   `useTransition`: Marks a state transition as non-blocking.
*   `useDeferredValue`: Defers updating a non-critical part of the UI.

### Other Hooks
These are mostly used by library authors.
*   `useDebugValue`: Customizes the label in React DevTools for custom hooks.
*   `useId`: Generates a unique ID (for accessibility APIs).
*   `useSyncExternalStore`: Subscribes to an external store.
*   `useActionState`: Manages state of actions.
