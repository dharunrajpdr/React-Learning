# 📘 React – Event Handling

## 1. What is Event Handling?

**Event handling** means responding to actions performed by the user.

Examples:

```text
Click
Typing
Submit
Mouse movement
Keyboard press
Change
Focus
```

### Simple Definition

> Event handling in React is the process of responding to user actions using event handler functions.

---

# 2. Common React Events

| Event | Used For |
|---|---|
| `onClick` | Button clicks |
| `onChange` | Input value changes |
| `onSubmit` | Form submission |
| `onMouseEnter` | Mouse enters an element |
| `onMouseLeave` | Mouse leaves an element |
| `onKeyDown` | Keyboard key pressed |
| `onFocus` | Element gets focus |
| `onBlur` | Element loses focus |

---

# 3. `onClick`

`onClick` is used when the user clicks an element.

```jsx
function App() {

    function handleClick() {
        alert("Button clicked!");
    }

    return (
        <button onClick={handleClick}>
            Click Me
        </button>
    );
}
```

---

# 4. Inline Event Handler

We can also write the function directly.

```jsx
function App() {

    return (
        <button onClick={() => alert("Hello!")}>
            Click Me
        </button>
    );
}
```

---

# 5. Important: Function vs Function Call

### ✅ Correct

```jsx
<button onClick={handleClick}>
    Click
</button>
```

### ❌ Usually Wrong

```jsx
<button onClick={handleClick()}>
    Click
</button>
```

Why?

```text
handleClick
    ↓
Passes the function

handleClick()
    ↓
Calls the function immediately
```

---

# 6. Event Object

React provides an **event object** containing information about the event.

```jsx
function App() {

    function handleClick(event) {
        console.log(event);
    }

    return (
        <button onClick={handleClick}>
            Click
        </button>
    );
}
```

---

# 7. `onChange`

`onChange` is commonly used with input fields.

```jsx
function App() {

    function handleChange(event) {
        console.log(event.target.value);
    }

    return (
        <input onChange={handleChange} />
    );
}
```

If the user types:

```text
Dharun
```

The value can be accessed using:

```jsx
event.target.value
```

---

# 8. Event Handling with State

Event handling is commonly combined with state.

```jsx
import { useState } from "react";

function App() {

    const [name, setName] = useState("");

    function handleChange(event) {
        setName(event.target.value);
    }

    return (
        <div>

            <input
                value={name}
                onChange={handleChange}
            />

            <h2>Hello {name}</h2>

        </div>
    );
}
```

### Flow

```text
User types
    ↓
onChange
    ↓
handleChange()
    ↓
setName()
    ↓
State changes
    ↓
UI updates
```

---

# 9. `onSubmit`

Used when submitting a form.

```jsx
function App() {

    function handleSubmit(event) {

        event.preventDefault();

        console.log("Form submitted");
    }

    return (
        <form onSubmit={handleSubmit}>

            <input type="text" />

            <button type="submit">
                Submit
            </button>

        </form>
    );
}
```

---

# 10. `preventDefault()`

Normally, submitting an HTML form can cause the browser's default form behavior.

In React, we can prevent it using:

```jsx
event.preventDefault();
```

Example:

```jsx
function handleSubmit(event) {

    event.preventDefault();

    console.log("Form submitted");
}
```

This is commonly used when handling forms with React.

---

# 11. Passing Arguments to Event Handlers

Suppose we want to pass an ID.

```jsx
function deleteUser(id) {
    console.log(id);
}
```

We can use:

```jsx
<button onClick={() => deleteUser(101)}>
    Delete
</button>
```

### Flow

```text
Click
 ↓
Arrow Function
 ↓
deleteUser(101)
```

---

# 12. Multiple Buttons

```jsx
function App() {

    function handleClick(name) {
        alert(name);
    }

    return (
        <div>

            <button onClick={() => handleClick("Dharun")}>
                Dharun
            </button>

            <button onClick={() => handleClick("Ajay")}>
                Ajay
            </button>

        </div>
    );
}
```

---

# 13. Mouse Events

Example:

```jsx
function App() {

    function handleMouseEnter() {
        console.log("Mouse entered");
    }

    return (
        <div onMouseEnter={handleMouseEnter}>
            Move mouse here
        </div>
    );
}
```

---

# 14. Keyboard Events

### `onKeyDown`

```jsx
function App() {

    function handleKeyDown(event) {
        console.log(event.key);
    }

    return (
        <input onKeyDown={handleKeyDown} />
    );
}
```

If the user presses:

```text
Enter
```

Then:

```text
event.key
```

returns:

```text
Enter
```

---

# 15. React Event Naming

React uses **camelCase** for event names.

### HTML

```html
<button onclick="handleClick()">
```

### React

```jsx
<button onClick={handleClick}>
```

Examples:

```text
onclick     → onClick
onchange    → onChange
onsubmit    → onSubmit
onkeydown   → onKeyDown
```

---

# 16. Event Handling with Props

A parent can pass an event handler to a child using props.

### Parent

```jsx
function App() {

    function handleClick() {
        alert("Clicked");
    }

    return <Button onClick={handleClick} />;
}
```

### Child

```jsx
function Button({ onClick }) {

    return (
        <button onClick={onClick}>
            Click Me
        </button>
    );
}
```

This allows the child to trigger behavior defined by the parent.

---

# 🧠 Easy Memory Trick

```text
User Action
    ↓
React Event
    ↓
Event Handler
    ↓
Function
    ↓
State / UI Update
```

---

# 🎯 Interview One-Liners

### What is event handling in React?

> Event handling is the process of responding to user actions such as clicks, typing, and form submissions.

### How do you handle a click in React?

```jsx
<button onClick={handleClick}>
```

### Why is `onClick={handleClick}` preferred over `onClick={handleClick()}`?

> `onClick={handleClick}` passes the function, while `handleClick()` calls the function immediately during rendering.

### What is `event`?

> The event object contains information about the user interaction that triggered the event.

### What is `event.target.value`?

> It gives the current value of the element that triggered the event, commonly used with input fields.

### What does `preventDefault()` do?

> It prevents the browser's default behavior for an event.

---

# 🔥 Quick Revision

```text
Event Handling
→ Respond to user actions

onClick
→ Click

onChange
→ Input changes

onSubmit
→ Form submission

onKeyDown
→ Keyboard press

event.target.value
→ Input value

event.preventDefault()
→ Prevent default browser behavior
```

### Basic Example

```jsx
function App() {

    function handleClick() {
        console.log("Clicked");
    }

    return (
        <button onClick={handleClick}>
            Click Me
        </button>
    );
}
```

# ⭐ Key Point

> **React event handling connects user actions such as clicks, typing, and form submission to JavaScript functions that perform the required action.**
