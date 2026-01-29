# JSX (JavaScript XML)

JSX is a syntax extension for JavaScript. It allows you to describe what your UI should look like in a visual way that's easy for developers to read, while keeping all the power of JavaScript logic in the same place.

## 1. It’s Not Actually HTML
While JSX looks like HTML, the browser cannot read it. If you try to run a `.jsx` file directly in Chrome, it will throw a syntax error.

**Under the hood**: A tool like Babel or SWC (the compilers) takes your JSX and transforms it into regular JavaScript objects that the React engine understands.

**What you write:**
```javascript
const element = <h1 className="title">Hello World</h1>;
```

**What the browser actually sees (after compilation):**
```javascript
const element = React.createElement(
  'h1',
  { className: 'title' },
  'Hello World'
);
```

## 2. The Power of "The Curly Braces"
The real magic of JSX is that you can "escape" back into JavaScript at any time by using `{}`. This is how you make your UI dynamic.

```javascript
const name = "World";
const element = <h1>Hello, {name}</h1>; // Renders: Hello, World
```

Inside those braces, you can put **any valid JavaScript expression**:
- **Math**: `<span>{2 + 2}</span>`
- **Function calls**: `<div>{calculateTotal()}</div>`
- **Ternary operators**: `<p>{isLoggedIn ? "Welcome back!" : "Please Log In"}</p>`

## 3. Key Differences from HTML
Because JSX is actually JavaScript, there are places where the syntax differs from standard HTML:

1.  **`className` instead of `class`**: Since `class` is a reserved keyword in JavaScript (for ES6 classes), React uses `className`.
2.  **camelCase for everything**: Attributes like `onclick` become `onClick`, and `tabindex` becomes `tabIndex`.
3.  **The Single Parent Rule**: A JSX expression must return exactly one parent element. You cannot return two `<h1>` tags side-by-side without wrapping them in a `<div>` or a Fragment (`<>...</>`).
    *   *Why?* JSX turns into a function call (`React.createElement`). In JavaScript, a function cannot return two values at once; it has to return one object or array.

## 4. Why Use JSX?
Before JSX, developers used to have to write long, messy strings of HTML inside their JS or manually create every single DOM node.

JSX solves three big problems:
*   **Visual Clarity**: It’s much easier to see the structure of your UI.
*   **Safety**: React DOM escapes any values embedded in JSX before rendering them. This prevents **XSS (Cross-Site Scripting)** attacks because everything is converted to a string before being rendered.
*   **Developer Experience**: Modern IDEs can give you autocomplete and error highlighting for your "HTML" because it's technically part of your JS code.
