# Critical Rendering Path (CRP)

The Critical Rendering Path (CRP) is the sequence of steps the browser takes
to turn your HTML, CSS, and JavaScript files into pixels on the screen. If any
step is slow, the page feels slow.

## 1) The inputs: linking resources

### Linking CSS (styles)
CSS is linked in the `<head>` so the browser can download and parse it before
it paints anything.

```html
<link rel="stylesheet" href="styles.css">
```

**Behavior:** when the browser sees this tag, it requests the file.

**Why it goes in the head:** CSS is **render‑blocking**. The browser will not
paint until it has the CSS needed for the first render. This prevents a
Flash of Unstyled Content (FOUC), where the page appears broken and then snaps
into place.

### Linking JavaScript (logic)
JavaScript is linked with the `<script>` tag.

```html
<script src="app.js"></script>
```

**Behavior:** when the HTML parser reaches a normal script, it **pauses**
building the DOM, downloads the script, executes it, and only then continues.

**Why it goes after content:** JavaScript is **parser‑blocking**. If you put it
early, it can delay the HTML from being parsed and the content from appearing.

**Modern fix:** load scripts without blocking parsing.

```html
<script defer src="app.js"></script>
```

`defer` downloads in the background and runs the script only after the HTML
is fully parsed.

**When to use `async`:**  
Use the `async` attribute for scripts that do **not** affect the page layout or DOM, such as analytics and advertising tags. These scripts download and execute as soon as they're ready, without blocking HTML parsing **or** guaranteeing order.

Example:

```html
<script async src="https://www.googletagmanager.com/gtag/js?id=GA_MEASUREMENT_ID"></script>
```

Scripts with `async` are best for third-party tools that can run at any time, since they may execute before or after other scripts, depending on download speed.

## 2) Build the trees

The browser builds two internal trees in parallel.

### DOM (Document Object Model)
The DOM represents the HTML structure, including all elements and their
parent/child relationships. It includes hidden elements too.

### CSSOM (CSS Object Model)
The CSSOM represents all styles and the cascade rules that decide which styles
apply to which elements.

## 3) Combine them: the Render Tree

The browser combines the DOM and CSSOM into the **Render Tree**:
- It walks the DOM and keeps only **visible** nodes.
- It matches each node with the final CSS rules from the CSSOM.

**Key distinction:**
- `opacity: 0` → still in the Render Tree (invisible but rendered)
- `display: none` → not in the Render Tree (removed from rendering)

## 4) Layout and Paint

### Layout (reflow)
The browser calculates the position and size of each render object inside the
viewport.

### Paint
The browser draws pixels: text, colors, images, borders, shadows, etc.

## Optimization cheat sheet
- **CSS:** keep it small; it blocks the first paint.
- **JS:** use `defer` or `async` to avoid blocking HTML parsing.
- **Images/fonts:** lazy‑load or preload critical assets when needed.