# Props vs State

If Components are the building blocks, **Props** and **State** are the data that flows through them.
**Props** are how components talk to each other, and **State** is how a component remembers things.

## 1. Props (Properties)
Props are arguments passed into a component from its parent, much like arguments passed into a function.

*   **Read-Only (Immutable)**: A component can never change its own props. It just receives them and displays them.
*   **Directional**: Data flows one way—from Parent to Child.
*   **Purpose**: To make components reusable.

```javascript
// Parent Component
const Gallery = () => {
  return <Image url="cat.jpg" caption="A cute cat" />; // Passing props
};

// Child Component
const Image = (props) => {
  // props.url is "cat.jpg"
  return (
    <figure>
      <img src={props.url} />
      <figcaption>{props.caption}</figcaption>
    </figure>
  );
};
```

## 2. State
State is a built-in object that stores data that changes over time. Unlike props, state is managed **inside** the component.

*   **Mutable**: The component can change its state whenever it wants.
*   **Private**: State is encapsulated. A parent doesn't know about its child's state unless the child shares it.
*   **Triggers Re-render**: Whenever the state changes, React automatically re-renders the component to show the new data.

### The Code (Using `useState`)

```javascript
import { useState } from 'react';

const LightSwitch = () => {
  const [isOn, setIsOn] = useState(false);

  return (
    <div>
      <p>The light is {isOn ? "ON" : "OFF"}</p>
      <button onClick={() => setIsOn(!isOn)}>
        Toggle Switch
      </button>
    </div>
  );
};
```
