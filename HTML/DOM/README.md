## DOM Basics

### What Exactly is the DOM?

When a browser loads a web page, it takes your static HTML and parses it into a
dynamic structure. This structure is the DOM.

The DOM is not your HTML file. It is an API (Application
Programming Interface) for valid HTML and XML documents. It represents the page
so that programs (like JavaScript) can change the document structure, style, and
content.

If HTML is the architectural blueprint, the DOM is the actual house that you
can walk into, open windows, and paint walls.

### The Tree Structure

The browser represents the DOM as a logical tree. Every piece of your
document — tags, text inside tags, attributes — becomes a node in this tree.

**The Main Node Types**

- The document node: The root of the tree. Everything branches from here.
- Element nodes: The HTML tags themselves (like `<div>`, `<body>`, `<h1>`).
- Text nodes: The actual text inside the tags.
- Attribute nodes: The properties of elements (like `class="hero"` or
  `src="img.jpg"`).

Note: In the DOM, text is a leaf node. It cannot have children.

### Why "Object" Model?

This is where JavaScript comes in. The DOM represents every node as a JavaScript
object.

Because they are objects, they have properties and methods. This is the
interface that allows JavaScript to talk to the HTML.

- Properties (state): `element.innerHTML`, `element.style.color`,
  `element.classList`
- Methods (actions): `element.remove()`, `element.appendChild()`,
  `element.addEventListener()`

### How We Manipulate the DOM

As a developer, you rarely touch the raw DOM tree manually. You use the DOM API
provided by the browser (usually via the `document` object in JavaScript).

**1) Select**

```javascript
// The old school way
const title = document.getElementById("main-title");

// The modern way (recommended)
const button = document.querySelector(".submit-btn");
const allListItems = document.querySelectorAll("li");
```

**2) Manipulate**

```javascript
// Change the content
title.innerText = "Welcome to the Matrix";

// Change the style
button.style.backgroundColor = "green";

// Add a class (best practice for styling)
button.classList.add("active");
```

**3) Listen**

```javascript
button.addEventListener("click", () => {
  alert("You clicked the DOM node!");
});
```

### The Cost of DOM Manipulation (Performance)

Touching the DOM is expensive.

Every time you change something in the DOM (like the width of an element), the
browser has to calculate the geometry of the page (reflow) and draw pixels to
the screen (repaint).

- Reflow: Calculating positions and sizes. High CPU cost.
- Repaint: Coloring the pixels.

Why React or Vue exist: Modern frameworks use a virtual DOM. They calculate
changes in memory first, and then update the real DOM in one efficient batch,
minimizing expensive reflows.
