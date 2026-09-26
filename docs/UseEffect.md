# 📘 React – useEffect Hook

## 1. What is `useEffect`?

`useEffect` is a React Hook used to perform **side effects** in a component.

### Simple Definition

> `useEffect` is used to run code after a component renders.

---

# 2. What is a Side Effect?

A **side effect** is an operation that interacts with something outside the normal rendering process.

Common examples:

```text
API calls
Timers
Event listeners
Updating document title
Subscriptions
Reading external data
```

---

# 3. Import `useEffect`

```jsx
import { useEffect } from "react";
```

---

# 4. Basic Syntax

```jsx
useEffect(() => {

    // Code to execute

}, []);
```

Structure:

```text
useEffect(
    effect function,
    dependency array
)
```

---

# 5. Simple Example

```jsx
import { useEffect } from "react";

function App() {

    useEffect(() => {
        console.log("Component rendered");
    }, []);

    return (
        <h1>Hello React</h1>
    );
}
```

Because the dependency array is empty:

```jsx
[]
```

the effect runs after the initial render.

---

# 6. `useEffect` Without Dependency Array

```jsx
useEffect(() => {
    console.log("Effect executed");
});
```

When there is **no dependency array**, the effect runs after every render.

Example:

```jsx
function App() {

    const [count, setCount] = useState(0);

    useEffect(() => {
        console.log("Effect executed");
    });

    return (
        <button onClick={() => setCount(count + 1)}>
            {count}
        </button>
    );
}
```

Every state update causes a render, so the effect runs again.

---

# 7. `useEffect` with Empty Dependency Array

```jsx
useEffect(() => {
    console.log("Effect executed");
}, []);
```

This is commonly used for work that should happen after the component's initial render.

Example:

```jsx
useEffect(() => {
    console.log("Fetch initial data");
}, []);
```

---

# 8. `useEffect` with Dependencies

We can specify values that the effect depends on.

```jsx
useEffect(() => {

    console.log("Count changed");

}, [count]);
```

The effect runs after the initial render and when `count` changes.

### Flow

```text
Initial render
      ↓
useEffect runs

count changes
      ↓
Component re-renders
      ↓
useEffect runs again
```

---

# 9. Dependency Array

The dependency array tells React **when the effect should run again**.

Example:

```jsx
useEffect(() => {
    console.log("Name changed");
}, [name]);
```

Here the effect depends on:

```text
name
```

If `name` changes:

```text
Effect runs
```

---

# 10. Three Common Patterns

### 1. No Dependency Array

```jsx
useEffect(() => {
    // runs after every render
});
```

### 2. Empty Dependency Array

```jsx
useEffect(() => {
    // runs after initial render
}, []);
```

### 3. Specific Dependencies

```jsx
useEffect(() => {
    // runs after initial render
    // and when count changes
}, [count]);
```

### Easy Table

| Code | Behavior |
|---|---|
| `useEffect(() => {})` | After every render |
| `useEffect(() => {}, [])` | After initial render |
| `useEffect(() => {}, [count])` | Initial render + when `count` changes |

---

# 11. Example with `useState`

```jsx
import { useState, useEffect } from "react";

function App() {

    const [count, setCount] = useState(0);

    useEffect(() => {
        console.log("Count:", count);
    }, [count]);

    return (
        <div>

            <h1>{count}</h1>

            <button onClick={() => setCount(count + 1)}>
                Increase
            </button>

        </div>
    );
}
```

If count changes:

```text
0 → 1
1 → 2
2 → 3
```

the effect runs after each update.

---

# 12. Updating Document Title

A simple real-world example:

```jsx
import { useState, useEffect } from "react";

function App() {

    const [count, setCount] = useState(0);

    useEffect(() => {
        document.title = `Count: ${count}`;
    }, [count]);

    return (
        <button onClick={() => setCount(count + 1)}>
            {count}
        </button>
    );
}
```

When count becomes:

```text
5
```

the browser tab title becomes:

```text
Count: 5
```

---

# 13. API Calls with `useEffect`

`useEffect` is commonly used for API calls.

```jsx
import { useEffect } from "react";

function App() {

    useEffect(() => {

        fetch("https://example.com/api/users")
            .then(response => response.json())
            .then(data => console.log(data));

    }, []);

    return <h1>Users</h1>;
}
```

The empty dependency array means the effect is set up for the initial render.

We will learn API calls in more detail later.

---

# 14. Cleanup Function

Some effects create resources that need to be cleaned up.

Examples:

```text
Timer
Event listener
Subscription
WebSocket connection
```

`useEffect` can return a cleanup function.

```jsx
useEffect(() => {

    console.log("Effect started");

    return () => {
        console.log("Cleanup");
    };

}, []);
```

---

# 15. Timer Example

```jsx
import { useEffect } from "react";

function App() {

    useEffect(() => {

        const timer = setInterval(() => {
            console.log("Running...");
        }, 1000);

        return () => {
            clearInterval(timer);
        };

    }, []);

    return <h1>Timer Example</h1>;
}
```

### Flow

```text
Component renders
      ↓
Timer starts
      ↓
Component is removed
      ↓
Cleanup runs
      ↓
Timer stops
```

---

# 16. Event Listener Cleanup

```jsx
useEffect(() => {

    function handleResize() {
        console.log(window.innerWidth);
    }

    window.addEventListener("resize", handleResize);

    return () => {
        window.removeEventListener("resize", handleResize);
    };

}, []);
```

This prevents the event listener from remaining unnecessarily after the component is removed.

---

# 17. Important: Don't Make the Effect Callback `async`

Avoid:

```jsx
useEffect(async () => {
    // ...
}, []);
```

Instead:

```jsx
useEffect(() => {

    async function fetchData() {
        const response = await fetch(
            "https://example.com/api/users"
        );

        const data = await response.json();

        console.log(data);
    }

    fetchData();

}, []);
```

---

# 18. `useEffect` vs `useState`

| `useState` | `useEffect` |
|---|---|
| Stores state | Performs side effects |
| Updates component data | Runs side-effect logic |
| Returns state + setter | Returns nothing useful for state |
| Example: count | Example: API call |

---

# 19. Common Uses of `useEffect`

```text
API Calls
    ↓
Fetch data

Timers
    ↓
setInterval / setTimeout

Event Listeners
    ↓
resize / keyboard events

Document Title
    ↓
document.title

Subscriptions
    ↓
Subscribe / cleanup
```

---

# 20. Common Mistake – Missing Dependencies

Example:

```jsx
useEffect(() => {
    console.log(count);
}, []);
```

If the effect is intended to react to changes in `count`, then `count` should generally be included:

```jsx
useEffect(() => {
    console.log(count);
}, [count]);
```

---

# 🧠 Easy Memory Trick

Remember:

```text
useState
→ Store data

useEffect
→ Do something after rendering
```

And:

```text
[] 
→ Initial effect

[count]
→ When count changes

No array
→ After every render
```

---

# 🎯 Interview One-Liners

### What is `useEffect`?

> `useEffect` is a React Hook used to perform side effects in functional components.

### What are side effects?

> Side effects are operations such as API calls, timers, subscriptions, event listeners, or updating the document title.

### What does an empty dependency array mean?

```jsx
useEffect(() => {
    // effect
}, []);
```

> The effect is set up to run after the initial render.

### What happens without a dependency array?

```jsx
useEffect(() => {
    // effect
});
```

> The effect runs after every render.

### What is a cleanup function?

> A cleanup function is used to remove or stop resources created by an effect, such as timers and event listeners.

### Why do we use dependencies?

> Dependencies tell React which values the effect uses and when the effect should run again.

---

# 🔥 Quick Revision

```text
useEffect
→ Handle side effects

Syntax:

useEffect(() => {
    // side effect
}, [dependencies]);

No dependency array
→ Every render

[]
→ Initial render

[count]
→ Initial render + count changes

return () => {}
→ Cleanup
```

# ⭐ Key Point

> **Use `useEffect` when your component needs to perform side effects such as API calls, timers, event listeners, subscriptions, or updating external systems.**
