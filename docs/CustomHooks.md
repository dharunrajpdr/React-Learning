# 📘 React – Custom Hooks

## 1. What is a Custom Hook?

A **Custom Hook** is a JavaScript function that:

- Uses React Hooks
- Contains reusable logic
- Can be shared between multiple components
- Usually starts with the word `use`

Examples:

```text
useFetch()
useAuth()
useForm()
useLocalStorage()
```

### Simple Definition

> A Custom Hook is a reusable function that contains React logic.

---

# 2. Why Do We Need Custom Hooks?

Suppose multiple components need the same logic.

Without a Custom Hook:

```text
Component A
    ↓
API logic

Component B
    ↓
Same API logic

Component C
    ↓
Same API logic
```

This causes **duplicate code**.

With a Custom Hook:

```text
             Custom Hook
            /     |     \
           ↓      ↓      ↓
      Component A B   Component C
```

The logic can be reused.

---

# 3. Basic Custom Hook Syntax

```jsx
function useSomething() {
  // React logic

  return something;
}
```

### Example

```jsx
import { useState } from "react";

function useCounter() {
  const [count, setCount] = useState(0);

  const increment = () => {
    setCount(count + 1);
  };

  return {
    count,
    increment
  };
}
```

---

# 4. Using a Custom Hook

```jsx
function App() {
  const { count, increment } = useCounter();

  return (
    <>
      <h1>{count}</h1>

      <button onClick={increment}>
        Increment
      </button>
    </>
  );
}
```

The component uses the logic without writing the state logic again.

---

# 5. Complete Example

### Custom Hook

```jsx
import { useState } from "react";

function useCounter() {
  const [count, setCount] = useState(0);

  const increment = () => {
    setCount(prev => prev + 1);
  };

  const decrement = () => {
    setCount(prev => prev - 1);
  };

  return {
    count,
    increment,
    decrement
  };
}

export default useCounter;
```

### Component

```jsx
import useCounter from "./useCounter";

function App() {
  const {
    count,
    increment,
    decrement
  } = useCounter();

  return (
    <>
      <h1>Count: {count}</h1>

      <button onClick={increment}>
        +
      </button>

      <button onClick={decrement}>
        -
      </button>
    </>
  );
}

export default App;
```

---

# 6. Custom Hook with useEffect

Custom Hooks can use other Hooks.

Example:

```jsx
import { useEffect, useState } from "react";

function useWindowWidth() {
  const [width, setWidth] = useState(window.innerWidth);

  useEffect(() => {
    const handleResize = () => {
      setWidth(window.innerWidth);
    };

    window.addEventListener("resize", handleResize);

    return () => {
      window.removeEventListener("resize", handleResize);
    };
  }, []);

  return width;
}
```

### Using it

```jsx
function App() {
  const width = useWindowWidth();

  return <h1>Width: {width}</h1>;
}
```

Now any component can reuse this logic.

---

# 7. Custom Hook for API Calls

A common real-world use case is API fetching.

```jsx
import { useEffect, useState } from "react";

function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchData = async () => {
      try {
        const response = await fetch(url);

        if (!response.ok) {
          throw new Error("Failed to fetch data");
        }

        const result = await response.json();

        setData(result);
      } catch (err) {
        setError(err.message);
      } finally {
        setLoading(false);
      }
    };

    fetchData();
  }, [url]);

  return {
    data,
    loading,
    error
  };
}

export default useFetch;
```

### Component

```jsx
function App() {
  const {
    data,
    loading,
    error
  } = useFetch("https://example.com/api/users");

  if (loading) {
    return <p>Loading...</p>;
  }

  if (error) {
    return <p>{error}</p>;
  }

  return <pre>{JSON.stringify(data, null, 2)}</pre>;
}
```

---

# 8. Rules of Custom Hooks

Custom Hooks follow the **Rules of Hooks**.

### Rule 1: Name should start with `use`

✅ Correct:

```jsx
useFetch()
useCounter()
useAuth()
```

❌ Avoid:

```jsx
fetchData()
counter()
```

if they are intended to be Hooks.

---

### Rule 2: Call Hooks only at the top level

✅ Correct:

```jsx
function useCounter() {
  const [count, setCount] = useState(0);

  return count;
}
```

❌ Avoid:

```jsx
if (condition) {
  const [count, setCount] = useState(0);
}
```

Hooks should not be called conditionally.

---

### Rule 3: Custom Hooks can call other Hooks

For example:

```jsx
function useSomething() {
  const [value, setValue] = useState("");

  useEffect(() => {
    // logic
  }, []);

  return value;
}
```

---

# 9. Custom Hook vs Normal Function

| Feature | Custom Hook | Normal Function |
|---|---|---|
| Uses React Hooks | ✅ Yes | Usually ❌ |
| Name convention | Starts with `use` | Any valid name |
| Reuses React logic | ✅ Yes | Not specifically |
| Can use `useState` | ✅ Yes | ❌ Not as an ordinary function |
| Can use `useEffect` | ✅ Yes | ❌ Not as an ordinary function |

### Simple Memory

```text
Normal Function
→ Reusable JavaScript logic

Custom Hook
→ Reusable React logic
```

---

# 10. Custom Hook vs Component

### Component

Returns **JSX**:

```jsx
function Welcome() {
  return <h1>Hello</h1>;
}
```

### Custom Hook

Usually returns **data/functions/state**:

```jsx
function useCounter() {
  const [count, setCount] = useState(0);

  return {
    count,
    setCount
  };
}
```

### Remember

```text
Component → UI
Custom Hook → Logic
```

---

# 11. Common Real-World Custom Hooks

Some commonly created Custom Hooks are:

```text
useFetch()
    → API requests

useAuth()
    → Authentication logic

useForm()
    → Form handling

useLocalStorage()
    → Local storage management

useDebounce()
    → Delay frequent input changes

useWindowSize()
    → Window dimensions

useToggle()
    → true / false state
```

---

# 12. Example: useToggle()

```jsx
import { useState } from "react";

function useToggle(initialValue = false) {
  const [value, setValue] = useState(initialValue);

  const toggle = () => {
    setValue(prev => !prev);
  };

  return [value, toggle];
}
```

### Usage

```jsx
function App() {
  const [isOpen, toggle] = useToggle(false);

  return (
    <>
      <button onClick={toggle}>
        Toggle
      </button>

      {isOpen && <p>Hello React!</p>}
    </>
  );
}
```

---

# 13. Important Point

Custom Hooks **share logic, not state itself**.

For example:

```jsx
function ComponentA() {
  const count = useCounter();
}

function ComponentB() {
  const count = useCounter();
}
```

Each component gets its **own instance of the hook's state**.

```text
Component A
    ↓
useCounter()
    ↓
Own state

Component B
    ↓
useCounter()
    ↓
Own state
```

If you need truly shared state between components, consider:

```text
Context API
State management library
Lifting state up
```

---

# 14. Benefits of Custom Hooks

### ✅ Reusability

Write logic once and reuse it.

### ✅ Cleaner Components

Components can focus more on UI.

### ✅ Less Duplicate Code

Avoid repeating the same logic.

### ✅ Better Organization

Complex logic can be moved into separate files.

### ✅ Easier Maintenance

Changes can be made in one place.

---

# 15. Typical File Structure

```text
src/
├── components/
│   ├── User.jsx
│   └── Product.jsx
│
├── hooks/
│   ├── useFetch.js
│   ├── useAuth.js
│   └── useToggle.js
│
├── App.jsx
└── main.jsx
```

A common convention is to keep Custom Hooks inside:

```text
src/hooks/
```

---

# 16. Interview Questions

### Q1. What is a Custom Hook?

A Custom Hook is a reusable function that contains React Hook logic.

### Q2. Why do we use Custom Hooks?

To reuse stateful or other React-related logic across components and reduce duplicate code.

### Q3. What naming convention is used?

Custom Hook names should start with:

```text
use
```

Example:

```jsx
useFetch()
```

### Q4. Can a Custom Hook use other Hooks?

Yes.

Example:

```jsx
useState()
useEffect()
useRef()
useContext()
```

can be used inside a Custom Hook, following the Rules of Hooks.

### Q5. Do Custom Hooks share state?

No. Calling the same Custom Hook from different components normally creates separate state for each component.

### Q6. What is the difference between a Custom Hook and a component?

```text
Component → mainly returns UI/JSX
Custom Hook → mainly returns reusable logic/data
```

---

# 17. Quick Revision

```text
Custom Hook
     ↓
Reusable React Logic
     ↓
Function starting with "use"
     ↓
Can use other Hooks
     ↓
Used by multiple components
```

### Example

```jsx
function useCounter() {
  const [count, setCount] = useState(0);

  return { count, setCount };
}
```

---

# 🎯 Interview One-Liner

> A Custom Hook is a reusable JavaScript function that uses React Hooks to share stateful or other React-related logic between components.

# ⭐ Key Point

```text
Component     → UI
Custom Hook   → Reusable React Logic
Normal Func   → Reusable JavaScript Logic
```
