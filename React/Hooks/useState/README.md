# The `useState` Hook

In React, `useState` allows a component to "remember" a value even after it has finished rendering.

## 1. The Basic Syntax
```javascript
const [state, setState] = useState(initialValue);
```
*   **`state`**: The current value you want to keep track of.
*   **`setState`**: The "dispatcher" function you use to update that value.
*   **`initialValue`**: The value the state starts with on the **first** render.

## 2. How useState works
`useState` isn't just a variable; it's a trigger for React's reconciliation engine.

### A. State Updates are Asynchronous
When you call `setState`, React doesn't change the value immediately. It schedules an update for the **next** render.

```javascript
const [count, setCount] = useState(0);

const handleClick = () => {
  setCount(count + 1);
  console.log(count); // This will still print 0!
};
```

You cannot read the "new" state in the same function where you set it. If you need to do something with the new value, use `useEffect` or calculate it in a local variable first.

### B. The Functional Update Pattern
If you need to update state based on the previous state (like a counter), passing the value directly can lead to race conditions if multiple updates happen quickly or in batch.

**The Problem**:
```javascript
// Suppose age is 42
function handleClick() {
  setAge(age + 1); // sets to 43
  setAge(age + 1); // sets to 43 (still reads old age)
  setAge(age + 1); // sets to 43
}
// Result: age is 43, not 45.
```

**The Solution**: Pass an **updater function**.
```javascript
function handleClick() {
  setAge(a => a + 1); // 42 => 43
  setAge(a => a + 1); // 43 => 44
  setAge(a => a + 1); // 44 => 45
}
```
React puts your updater functions in a queue and runs them sequentially during the next render.

### C. State Objects Must Be Replaced, Not Mutated
React uses **Shallow Comparison** (referential equality) to decide if it should re-render. If you mutate an object or array, the "address" in memory stays the same, and React won't see the change.

```javascript
const [user, setUser] = useState({ name: 'Taylor', age: 42 });

// WRONG: React won't re-render
user.name = 'Alex'; 
setUser(user);

// RIGHT: Spread the old object into a brand new one
setUser({ ...user, name: 'Alex' });
```

## 3. Performance: The "Lazy" Initialization
React saves the initial state once and ignores it on subsequent renders. However, if your initial value comes from an expensive calculation, you don't want to run that calculation on every render.

**The Wasteful Way**:
```javascript
// createInitialTodos() runs on EVERY render, even though result is ignored after first
const [todos, setTodos] = useState(createInitialTodos());
```

**The Efficient Way (Lazy)**:
```javascript
// Passing the function itself. React only calls it during initialization.
const [todos, setTodos] = useState(createInitialTodos);
```

## 4. Resetting State with `key`
Sometimes you want to completely wipe a component's state and start over. You can do this by changing its unique `key` prop.

When the key changes, React destroys the old component instance and creates a brand new one from scratch.

```javascript
export default function App() {
  const [version, setVersion] = useState(0);

  return (
    <>
      <button onClick={() => setVersion(version + 1)}>Reset Form</button>
      {/* Changing the key forces Form to unmount and remount */}
      <Form key={version} />
    </>
  );
}
```
