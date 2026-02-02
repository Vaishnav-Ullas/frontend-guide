# useReducer Hook

`useReducer` is a React Hook used for state management. While `useState` is perfect for simple values (toggles, inputs), `useReducer` works best for **complex state logic** that involves multiple sub-values or when the next state depends on the previous one.

It solves the problem of scattered state updates by moving all state modification logic into a single, organized function called a **reducer** (external to your component).

## 1. The Core Anatomy

The hook follows a specific signature:

```javascript
const [state, dispatch] = useReducer(reducer, initialArg, init?);
```

- **`reducer`**: A pure function that takes the `(state, action)` and determines the *new* state.
- **`initialArg`**: The initial state value (e.g., `{ count: 0 }`).
- **`init`** *(optional)*: A function for lazy initialization (useful for expensive calculations).
- **`state`**: The current snapshot of your data.
- **`dispatch`**:A function you call to trigger an update.

## 2. How the Cycle Works

Unlike `useState`, you typically don't update data directly. You send signals (actions) describing *what happened*.

1.  **User Action**: User clicks a button (e.g., "Delete").
2.  **Dispatch**: You call `dispatch({ type: 'deleted_item', id: 42 })`.
3.  **Reducer Runs**: React runs your reducer function with the current state and the action.
4.  **Update**: The reducer returns a *new* state object, causing the UI to re-render.

## 3. The Reducer Pattern

Reducers traditionally use a `switch` statement to handle different action types. This makes the logic readable and self-documenting.

```javascript
function reducer(state, action) {
  switch (action.type) {
    case 'changed_name': {
      return {
        ...state,          // 1. Copy the old state 
        name: action.nextName // 2. Update specific field
      };
    }
    case 'incremented_age': {
      return {
        ...state,
        age: state.age + 1 // Depends on previous state
      };
    }
    default: {
      throw Error('Unknown action: ' + action.type);
    }
  }
}
```

## 4. `useState` vs. `useReducer`

| Feature | `useState` | `useReducer` |
| :--- | :--- | :--- |
| **Complexity** | Best for primitives (strings, numbers). | Best for objects, arrays, or deep structures. |
| **Logic Location** | Inside event handlers. | Outside the component (Cleaner, Reusable). |
| **Updates** | Updates one variable at a time. | Can update multiple fields (e.g., name + age) in one action. |
| **Testing** | Harder to isolate logic from UI. | Very easy to unit test the reducer function in isolation. |

## 5. Use Cases

### A. Centralized Logic
If you have a form with 10+ fields, managing 10 `useState` hooks is tedious.
- **Problem**: Resetting the form requires calling 10 setters (`setName('')`, `setAge(0)`, etc.).
- **Solution**: With `useReducer`, a `reset_form` action can return the initial form object in one line. It keeps all form data synchronized.

### B. Dependent State Calculations
This is the most critical use case. Sometimes changing one value (e.g., `cartItems`) requires recalculating another (e.g., `totalPrice`).
- **Problem**: With `useState`, you might update `items` but forget to update `totalPrice`, leading to "stale state" bugs.
- **Solution**: The reducer has a "birds-eye view" of the entire state. You can update both in a single pass.

```javascript
// Example: Shopping Cart Reducer
case 'add_item': {
  const newItems = [...state.items, action.item];
  // Calculate dependent state immediately
  const newTotal = newItems.reduce((sum, item) => sum + item.price, 0);

  return {
    ...state,
    items: newItems,
    totalPrice: newTotal, 
    isFreeShipping: newTotal > 50 // Derived logic
  };
}
```
