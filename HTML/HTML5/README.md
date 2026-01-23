
# HTML5 and Major HTML5 APIs

HTML5 is the modern HTML standard. It expanded HTML with new elements and
introduced a large set of browser APIs that JavaScript can call to access
storage, hardware, media, graphics, and more.

An API (Application Programming Interface) here means built-in browser
features exposed to JavaScript.

## What changed with HTML5

Before HTML5, the browser was mostly a document viewer. With HTML5, the browser
became a platform:
- Store data without cookies
- Access device features (like location)
- Play audio/video without plugins
- Draw high-performance graphics
- Build rich web apps without extra install

## Major HTML5 APIs

### 1) Web Storage API (localStorage / sessionStorage)
Replaces many cookie use-cases. Faster and larger than cookies, and not sent
with every HTTP request.

```js
// Save data
localStorage.setItem("username", "DevMaster99");

// Read data
const user = localStorage.getItem("username");

// Remove data
localStorage.removeItem("username");
```

- `localStorage` -> persists until cleared (roughly 5-10 MB)
- `sessionStorage` -> persists until the tab is closed

### 2) Geolocation API
Lets you request the user’s physical location (GPS/Wi-Fi).
Permission is required every time unless the user allows it.

```js
if (navigator.geolocation) {
  navigator.geolocation.getCurrentPosition((position) => {
    const lat = position.coords.latitude;
    const long = position.coords.longitude;
    console.log(`You are at: ${lat}, ${long}`);
  });
} else {
  console.log("Geolocation is not supported by this browser.");
}
```

### 3) Canvas API
The `<canvas>` element is a drawable area that you control with JavaScript.
It is hardware-accelerated and used for games, charts, and animations.

```html
<canvas id="myGame" width="200" height="100"></canvas>

<script>
  const c = document.getElementById("myGame");
  const ctx = c.getContext("2d");

  ctx.fillStyle = "#FF0000";
  ctx.fillRect(0, 0, 150, 75);
</script>
```

### 4) Drag and Drop API
Lets you drag elements inside the browser without extra libraries.

```html
<div id="card" draggable="true">Drag me</div>
```

```js
const card = document.getElementById("card");

card.addEventListener("dragstart", (event) => {
  event.dataTransfer.setData("text/plain", "card");
});
```

### 5) Audio and Video APIs
Native media playback without Flash. The HTML tags are `<audio>` and `<video>`,
and JavaScript can control playback.

```html
<video width="320" height="240" controls>
  <source src="movie.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>
```

```js
const video = document.querySelector("video");
video.play();
video.pause();
video.currentTime = 10;
```
