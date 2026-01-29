# What is React?

React is a **JavaScript library for building user interfaces**. It was created by Facebook (now Meta) in 2013 because managing the UI of large apps (like Facebook and Instagram) with vanilla DOM manipulation was becoming unmanageable.

## 1) Core philosophy: everything is a component

In HTML you write a page. In React you build a **component tree**.

Think of it like LEGO: you build from small, reusable pieces instead of one big block.

- You have a `Button` brick.
- You have a `Navbar` brick.
- You have a `UserProfile` brick.

You combine these small, isolated pieces to build complex UIs.

**Why this helps:**

- **Reusability:** Write `<Button>` once, use it many times.
- **Isolation:** If `Navbar` breaks, it does not have to take down the whole app.

## 2) Declarative vs imperative

**Vanilla JS is imperative:** you tell the browser *how* to do things step by step.

**React is declarative:** you describe *what* the UI should look like for a given state. React updates the DOM to match.

**Imperative (old school):**  
“Find the button with ID `submit`. If the user is logged in, remove the `disabled` class and change the text to ‘Submit’.”

**React (declarative):**  
“The button is enabled when `isLoggedIn` is true.”

You update **state** (data), and React updates the UI. You do not manually add/remove classes or touch the DOM directly.

## 3) The Virtual DOM

The real DOM is slow: every change can trigger layout (reflow) and repaint. React uses a **Virtual DOM** to minimize real DOM updates.

1. **Copy:** React keeps a lightweight representation of the DOM in memory.
2. **Diff:** When state changes, React updates the Virtual DOM first (cheap).
3. **Reconciliation:** It compares the new Virtual DOM with the previous one and computes the **minimal set of changes**.
4. **Batch update:** It applies those changes to the real DOM in one optimized batch.

Result: fewer DOM operations and better performance for complex UIs.

## 4) JSX: HTML-like syntax in JavaScript

React uses **JSX** (JavaScript XML): syntax that looks like HTML but lives inside JavaScript.

**Vanilla JS:**

```js
const btn = document.createElement("button");
btn.innerText = "Click Me";
btn.className = "primary-btn";
document.body.appendChild(btn);
```

**React (JSX):**

```jsx
const Button = () => {
  return <button className="primary-btn">Click Me</button>;
};
```

JSX is transformed by a build step (e.g. Babel) into `React.createElement(...)` calls. You use `className` instead of `class` because `class` is reserved in JavaScript.

## Summary

- **React** = library for building UIs with components, state, and a declarative model.
- **Components** = reusable, isolated pieces that form a tree.
- **Declarative** = describe the UI for the current state; React updates the DOM.
- **Virtual DOM** = in-memory copy + diff + minimal updates to the real DOM.
- **JSX** = HTML-like syntax in JS, compiled to React elements.
