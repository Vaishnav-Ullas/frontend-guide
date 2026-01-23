## HTML Basics

### What is HTML?

HTML (HyperText Markup Language) is the standard markup language for documents
designed to be displayed in a web browser.

It is important to make a distinction here: HTML is not a programming language
(it doesn't have logic, loops, or functions). It is a markup language. It tells
the browser how to structure your content.

Think of a webpage like a human body:

- HTML is the skeleton: It provides the structure and holds everything together.
- CSS is the skin or clothing: It makes the structure look good (styling).
- JavaScript is the muscles or nervous system: It makes the structure actually
  do things (interactivity).

### The Building Blocks: Tags and Elements

HTML is written using elements, which are usually wrapped in tags.

- Tags: Keywords surrounded by angle brackets, like `<p>` (paragraph) or `<h1>`
  (heading). Most tags come in pairs: an opening tag and a closing tag (with a
  forward slash, e.g., `</p>`).
- Elements: The entire package — the opening tag, the content inside, and the
  closing tag.
- Attributes: Extra information provided inside the opening tag, like `class`,
  `id`, or `src`.

Example:

```html
<a href="https://google.com">Click Here</a>
```

- `<a>`: The tag (anchor)
- `href="..."`: The attribute (destination)
- `Click Here`: The content

### The Document Structure

Every HTML file follows a hierarchy, often called the DOM (Document Object
Model) tree. It looks a bit like this:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>My Awesome Blog</title>
  </head>
  <body>
    <h1>Hello, World!</h1>
    <p>This is my first paragraph.</p>
  </body>
</html>
```

### Why Semantic HTML Matters

You might see developers using `<div>` for everything. A `<div>` is a generic
container. While it works, modern web development relies on semantic HTML.

This means using tags that explain what the content is.

- Instead of `<div class="header">`, use `<header>`.
- Instead of `<div class="article">`, use `<article>`.
- Instead of `<div class="footer">`, use `<footer>`.

Why this matters:

- SEO: Search engines understand your content better.
- Accessibility: Screen readers rely on these tags to navigate the page for
  visually impaired users.
