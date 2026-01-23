## Semantic HTML

### What is Semantic HTML?

"Semantics" is the study of meaning. In HTML, a semantic element clearly
describes its meaning to both the browser and the developer.

- Non-semantic elements: `<div>` and `<span>` — they tell us nothing about their
  content.
- Semantic elements: `<form>`, `<table>`, `<article>`, `<header>` — these tags
  clearly define their content.

Think of it like labeling boxes in a warehouse:

- Non-semantic: A box labeled "Stuff."
- Semantic: A box labeled "Electronics - Q4 Inventory."

### Why Should You Care?

You might think, "If the user can't see the tags, why does it matter?"

- Accessibility (A11y): Screen readers use these tags to navigate. If you use
  `<div>` for a button, a screen reader just says "group." If you use
  `<button>`, it says "clickable button."
- SEO (Search Engine Optimization): Search engines use semantic tags to identify
  the main content vs. header or footer content.
- Maintainability: Semantic tags make your code easier to read and update later.

### The Major Structural Tags

**1) `<header>`**

Represents introductory content, not just the top of the page.

Usage: Logo, search bar, navigation.

Note: You can have multiple headers — an `<article>` can have its own header.

**2) `<nav>`**

Reserved for major navigation blocks.

Usage: Main menu, table of contents, footer links.

Don't use it for a single link to an external site.

**3) `<main>`**

Specifies the dominant content of the `<body>`.

Golden rule: Only one `<main>` tag per page. It should not contain sidebars or
repeated navigation.

**4) `<section>` vs. `<article>`**

`<article>`: Use this for content that makes sense on its own.

Examples: A blog post, a forum comment, a news story.

`<section>`: Use this to group related content together.

Examples: A "Testimonials" section, a "Contact Us" section, "Chapter 1."

Dev tip: If you can't think of a title for the block of content, it probably
shouldn't be a `<section>`. Use a `<div>` instead.

**5) `<aside>`**

Content that is tangentially related to the content around it.

Usage: Sidebars, "You might also like" boxes, advertising slots.

**6) `<footer>`**

Footer for a section or page.

Usage: Copyright info, contact links, sitemaps.

### Text-Level Semantics

Semantics isn't just about layout; it's about text too.

**`<strong>` vs `<b>`**

- `<b>`: Makes text bold (visual only).
- `<strong>`: Indicates importance (semantic).

**`<em>` vs `<i>`**

- `<i>`: Makes text italic (visual only).
- `<em>`: Indicates stress emphasis (semantic).

### A "Before and After" Example

**The "Div Soup" (Bad)**

```html
<div class="header">
  <div class="nav">...</div>
</div>
<div class="main-content">
  <div class="blog-post">...</div>
  <div class="sidebar">...</div>
</div>
<div class="footer">...</div>
```

**The Semantic Way (Good)**

```html
<header>
  <nav>...</nav>
</header>
<main>
  <article>...</article>
  <aside>...</aside>
</main>
<footer>...</footer>
```
