# Components

At its core, a **Component** in React is simply a **custom HTML element** that you build yourself. It is a reusable, self-contained piece of code that returns slightly different UI based on the data you feed it.

## 1. The Core Concept: "Building Blocks"
Think of components like LEGO bricks.
- **HTML** gives you the basic bricks: `<div>`, `<span>`, `<h1>`.
- **React Components** allow you to glue those basic bricks together to create new, complex bricks: `<Navbar />`, `<ProductCard />`, `<LikeButton />`.

## 2. Why Use Components?
Imagine you’re building a social media site. You have a "Post" feature that appears on the Home Feed, the Profile Page, and the Search Results.

### Without Components
You’d copy and paste the same HTML/CSS/JS in three different places.
*   **The Problem**: If you want to change the "Like" button color, you have to find and fix it in all three spots. If you miss one, your site looks inconsistent.

### With Components
You build one `<Post />` component and use it everywhere.
*   **The Solution**: If you update the code in that one file, the change reflects across the entire app instantly.

## 3. Functional vs. Class Components
In the history of React, there have been two main ways to write a component.

### Functional Components (The Modern Standard)
These are **plain JavaScript functions**. They are simpler, easier to read, and easier to test. Since React 16.8 (2019), this is the standard way to write React apps.

```javascript
const Welcome = (props) => {
  return <h1>Hello, {props.name}</h1>;
};
```

### Class Components (The "Legacy" Way)
Before 2019, if you wanted your component to have state or lifecycle methods, you had to use an ES6 Class.

```javascript
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

## 4. Why Did Class Components Become "Legacy"?
You’ll still see Class components in older projects, but almost all new React code uses Functions. Here’s why it shifted:

1.  **The "this" Keyword**: JavaScript's `this` is confusing. In Classes, you constantly had to "bind" methods to keep `this` context correct.
2.  **Boilerplate**: Classes require a lot of setup code (`constructor`, `super(props)`, `render()`). Functions are clean and minimal.
3.  **Hooks (The Game Changer)**: Originally, functions couldn't hold state. In 2019, React introduced **Hooks** (like `useState`). This gave functions all the powers of classes but with much cleaner code.
4.  **Minification**: Functions minify better than classes, resulting in slightly smaller and faster application bundles.

## 5. The Golden Rule: Capitalization
There is one strict rule you must follow: **React Components must start with a Capital Letter.**

*   `<div />` (lowercase): React treats this as a built-in HTML tag.
*   `<Welcome />` (Uppercase): React treats this as a custom component you wrote.

## 6. Pure Components

### Purity Principles
A React component should always exhibit **pure rendering logic**, meaning:
1.  **Same inputs, same output**: Given the same props, state, and context, it must consistently return the same JSX.
2.  **No side effects during rendering**: The component should not change any objects or variables that existed before it was called during its rendering process. Side effects (like network requests or modifying the DOM) belong in event handlers or `useEffect` hooks.

### Performance Optimization
The term "pure component" often specifically refers to the performance enhancement feature available in React.

#### Class Components
For class components, you can extend `React.PureComponent` instead of `React.Component`. A `PureComponent` automatically implements the `shouldComponentUpdate` lifecycle method, which performs a **shallow comparison** of the new and previous props and state. If the shallow comparison determines no changes, the component skips the re-render process.

#### Functional Components
For functional components, the equivalent optimization is achieved by wrapping the component in the `React.memo` higher-order component. `React.memo` also performs a shallow comparison of props to decide whether to skip re-rendering.

### Key Considerations
*   **Shallow Comparison**: The built-in optimization relies on shallow comparisons. This means if you **mutate objects or arrays** instead of creating new ones, the shallow comparison might miss the update, leading to unexpected behavior. It is crucial to **use immutable data practices** when working with pure components or `React.memo`.
*   **Modern React**: The official React documentation recommends using functional components and hooks over class components. While `PureComponent` is still supported, `React.memo` is the modern approach for achieving this performance optimization in function components.
*   **When to Use**: Use pure components or `React.memo` when you have components that **re-render frequently with the same props**, and the rendering logic is computationally expensive.
