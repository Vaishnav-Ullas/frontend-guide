# Meta Tags 

Meta tags live in the `<head>` of your document. Users never see them, but
machines do: browsers, search engines, and social platforms use them to decide
how to render, index, and preview your site.

## 1) The mobile must‑have: viewport

```html
<meta name="viewport" content="width=device-width, initial-scale=1">
```

**Why it matters:** mobile browsers default to a fake, wide desktop viewport
(often ~980px) and then scale it down. That makes pages look tiny and breaks
responsive CSS.

**Breakdown:**
- `width=device-width` → match the viewport to the device width so media
  queries work correctly.
- `initial-scale=1` → set the zoom to 1:1 so the page isn’t auto‑zoomed out.

**Bottom line:** if you want responsive design, this tag is mandatory.

## 2) Social sharing: Open Graph (OG)

Open Graph tags control how your link looks in Discord, LinkedIn, Facebook,
etc. Without them, you often get a plain blue link.

```html
<meta property="og:title" content="My Awesome Developer Blog">
<meta property="og:description" content="Learn HTML, CSS, and JS the right way.">
<meta property="og:image" content="https://mysite.com/assets/hero-banner.jpg">
<meta property="og:url" content="https://mysite.com">
```

**Tip:** `og:image` is the biggest click‑through driver. Use a dedicated
image, typically **1200×630**.

## 3) SEO classics: title + description

### `<title>`
Not technically a meta tag, but it lives in the `<head>`.

```html
<title>Learn Web Development | My Blog</title>
```

**Impact:** the blue link in Google results and the browser tab title.

### Meta description

```html
<meta name="description" content="A guide to the DOM and Critical Rendering Path.">
```

**Impact:** the snippet shown in search results (not a ranking factor by
itself, but it affects click‑through rate).

## 4) Character encoding: UTF‑8

Computers store text as numbers. The character encoding is the translation
table that maps numbers to letters and symbols.

**ASCII:** early 128‑character set (English‑only, very limited).  
**UTF‑8:** modern universal standard (all languages + emojis).

```html
<meta charset="UTF-8">
```

**Why you must declare it:**
- Avoids mojibake (garbled text like `CafÃ©`)
- Prevents incorrect re‑parsing mid‑document
- Reduces risk of encoding‑based security issues

**Rule:** make this the first tag in `<head>` so the browser reads it before
any content.