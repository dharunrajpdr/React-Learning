# 📘 React – useRef Hook

## 1. What is `useRef`?

`useRef` is a React Hook used to:

- Access a DOM element directly
- Store a value that persists between renders
- Change a value without causing a re-render

### Simple Definition

> `useRef` creates a mutable reference whose value persists across renders without triggering a re-render when changed.

---

# 2. Import `useRef`

```jsx
import { useRef } from "react";
```

---

# 3. Basic Syntax

```jsx
const ref = useRef(initialValue);
```

Example:

```jsx
const countRef = useRef(0);
```

The value is accessed using:

```jsx
countRef.current
```

---

# 4. Accessing a DOM Element

One of the most common uses of `useRef` is accessing an HTML element.

```jsx
import { useRef } from "react";

function App() {

    const inputRef = useRef(null);

    function focusInput() {
        inputRef.current.focus();
    }

    return (
        <div>

            <input ref={inputRef} />

            <button onClick={focusInput}>
                Focus Input
            </button>

        </div>
    );
}
```

### Flow

```text
useRef()
   ↓
Create reference
   ↓
ref={inputRef}
   ↓
Connect to input
   ↓
inputRef.current
   ↓
Access DOM element
```

---

# 5. What is `.current`?

The actual value stored inside a ref is available through:

```jsx
ref.current
```

Example:

```jsx
const inputRef = useRef(null);
```

Initially:

```text
inputRef.current
→ null
```

After attaching it:

```jsx
<input ref={inputRef} />
```

`inputRef.current` refers to the input DOM element.

---

# 6. `useRef` Does Not Cause Re-render

Consider:

```jsx
const countRef = useRef(0);

countRef.current++;
```

The value changes, but React does **not** automatically re-render the component because of this change.

Compare:

```jsx
setCount(count + 1);
```

with:

```jsx
countRef.current++;
```

### Difference

```text
useState
→ Update state
→ Re-render

useRef
→ Update ref
→ No re-render
```

---

# 7. Storing a Previous Value

`useRef` can store a value between renders.

```jsx
import { useEffect, useRef, useState } from "react";

function App() {

    const [count, setCount] = useState(0);

    const previousCount = useRef();

    useEffect(() => {
        previousCount.current = count;
    }, [count]);

    return (
        <div>

            <h2>Current: {count}</h2>

            <h2>
                Previous: {previousCount.current}
            </h2>

            <button onClick={() => setCount(count + 1)}>
                Increase
            </button>

        </div>
    );
}
```

The ref remembers a value between renders.

---

# 8. `useRef` with Input

Example: Clear an input.

```jsx
import { useRef } from "react";

function App() {

    const inputRef = useRef(null);

    function clearInput() {
        inputRef.current.value = "";
    }

    return (
        <div>

            <input ref={inputRef} />

            <button onClick={clearInput}>
                Clear
            </button>

        </div>
    );
}
```

---

# 9. `useRef` with Focus

```jsx
function focusInput() {
    inputRef.current.focus();
}
```

Other common DOM methods:

```jsx
inputRef.current.focus();
inputRef.current.blur();
inputRef.current.value;
```

---

# 10. Storing a Timer ID

`useRef` can store values that need to persist but don't need to trigger UI updates.

```jsx
import { useRef } from "react";

function App() {

    const timerRef = useRef(null);

    function startTimer() {

        timerRef.current = setInterval(() => {
            console.log("Running...");
        }, 1000);
    }

    function stopTimer() {

        clearInterval(timerRef.current);
    }

    return (
        <div>

            <button onClick={startTimer}>
                Start
            </button>

            <button onClick={stopTimer}>
                Stop
            </button>

        </div>
    );
}
```

---

# 11. `useRef` vs `useState`

| `useRef` | `useState` |
|---|---|
| Stores mutable reference | Stores state |
| Changing `.current` doesn't re-render | Setter causes re-render |
| Can access DOM elements | Used for UI data |
| Value persists between renders | Value persists between renders |
| Useful for timers/previous values | Useful for displayed data |

### Easy Example

```text
Need to update UI?
→ useState

Need to remember a value without re-render?
→ useRef
```

---

# 12. `useRef` vs Normal Variable

### Normal Variable

```jsx
function App() {

    let count = 0;

    count++;

}
```

The variable does not reliably persist across renders.

### `useRef`

```jsx
const countRef = useRef(0);

countRef.current++;
```

The value persists across renders.

---

# 13. Common Uses of `useRef`

```text
DOM access
    ↓
Focus input

Previous value
    ↓
Remember previous state

Timer
    ↓
Store interval ID

External values
    ↓
Persist mutable values

Third-party libraries
    ↓
Access DOM elements
```

---

# 14. Important Rule

Don't use refs for data that needs to be displayed and updated in the UI.

### ❌ Not ideal

```jsx
const countRef = useRef(0);

countRef.current++;
```

if the goal is to display the changing count.

### ✅ Better

```jsx
const [count, setCount] = useState(0);

setCount(count + 1);
```

---

# 15. Ref Example with Form

```jsx
import { useRef } from "react";

function App() {

    const nameRef = useRef(null);

    function handleSubmit(event) {

        event.preventDefault();

        console.log(nameRef.current.value);
    }

    return (
        <form onSubmit={handleSubmit}>

            <input
                ref={nameRef}
                placeholder="Enter name"
            />

            <button type="submit">
                Submit
            </button>

        </form>
    );
}
```

This is an example of an **uncontrolled input**.

---

# 🧠 Easy Memory Trick

Remember:

```text
useState
→ UI data

useRef
→ Reference / persistent value
```

And:

```text
useRef()
   ↓
ref.current
   ↓
Access stored value
```

---

# 🎯 Interview One-Liners

### What is `useRef`?

> `useRef` is a React Hook used to store a mutable value that persists between renders without causing a re-render when changed.

### What is `.current`?

> `.current` contains the value stored inside a ref.

### Can `useRef` access DOM elements?

> Yes. We can attach a ref to a DOM element and access it through `ref.current`.

### Does changing a ref cause a re-render?

> No. Changing `ref.current` does not trigger a component re-render.

### When should you use `useRef` instead of `useState`?

> Use `useRef` when the value needs to persist between renders but changing it does not need to update the UI.

### What are common uses of `useRef`?

> DOM access, focusing inputs, storing timer IDs, remembering previous values, and integrating with external libraries.

---

# 🔥 Quick Revision

```text
useRef
→ Create a reference

Syntax:
const ref = useRef(initialValue);

Access:
ref.current

Common uses:
→ DOM access
→ Focus input
→ Previous value
→ Timer ID
→ Persistent mutable value

Important:
Changing ref.current
→ Does NOT cause re-render
```

# ⭐ Key Point

> **Use `useRef` when you need a persistent value or direct DOM reference without triggering a re-render when that value changes.**
