# 📘 React – useCallback

## 1. What is useCallback?

`useCallback` is a React Hook used to **memoize a function**.

It helps keep the **same function reference** between renders until its dependencies change.

### Simple Definition

> `useCallback` remembers a function so that React can reuse the same function reference when its dependencies have not changed.

---

# 2. Basic Syntax

```jsx
const functionName = useCallback(() => {
  // function logic
}, [dependencies]);
```

Example:

```jsx
const handleClick = useCallback(() => {
  console.log("Clicked");
}, []);
```

---

# 3. Why Do We Need useCallback?

Consider:

```jsx
function App() {
  const [count, setCount] = useState(0);

  const handleClick = () => {
    console.log("Clicked");
  };

  return <Child onClick={handleClick} />;
}
```

Every time `App` renders, a new function can be created:

```text
Render 1 → handleClick → Function A

Render 2 → handleClick → Function B

Render 3 → handleClick → Function C
```

Even though the function does the same thing, its reference is different.

This can matter when passing the function to a memoized child.

---

# 4. Using useCallback

```jsx
const handleClick = useCallback(() => {
  console.log("Clicked");
}, []);
```

Now React can reuse the same function reference across renders while the dependencies remain unchanged.

```text
Render 1 → Function A

Render 2 → Function A

Render 3 → Function A
```

---

# 5. Complete Example

```jsx
import { useCallback, useState } from "react";

const Child = React.memo(function Child({ onClick }) {
  console.log("Child rendered");

  return (
    <button onClick={onClick}>
      Click
    </button>
  );
});

function App() {
  const [count, setCount] = useState(0);

  const handleClick = useCallback(() => {
    console.log("Button clicked");
  }, []);

  return (
    <>
      <h1>{count}</h1>

      <button onClick={() => setCount(prev => prev + 1)}>
        Increase Count
      </button>

      <Child onClick={handleClick} />
    </>
  );
}

export default App;
```

Because `handleClick` keeps the same reference, `React.memo` can help the child skip unnecessary renders when its other props have not changed.

---

# 6. useCallback and React.memo

These two are often used together.

```text
Parent
   ↓
useCallback()
   ↓
Stable function reference
   ↓
React.memo
   ↓
Child
```

### Example

```jsx
const Child = React.memo(({ onClick }) => {
  return <button onClick={onClick}>Click</button>;
});
```

Parent:

```jsx
const handleClick = useCallback(() => {
  console.log("Clicked");
}, []);

<Child onClick={handleClick} />
```

---

# 7. Dependency Array

Example:

```jsx
const handleClick = useCallback(() => {
  console.log(count);
}, [count]);
```

Here:

```text
count changes
     ↓
New function created
```

If `count` doesn't change:

```text
Same function reference can be reused
```

---

# 8. useCallback with Multiple Dependencies

```jsx
const handleSubmit = useCallback(() => {
  console.log(name, email);
}, [name, email]);
```

If either:

```text
name changes
OR
email changes
```

the function is recreated.

If neither changes, React can reuse the previous function reference.

---

# 9. useCallback Does NOT Execute the Function

This is important.

```jsx
const handleClick = useCallback(() => {
  console.log("Clicked");
}, []);
```

`useCallback` returns the function.

It does **not** call it immediately.

You call it later:

```jsx
<button onClick={handleClick}>
  Click
</button>
```

---

# 10. useCallback vs Normal Function

### Normal function

```jsx
const handleClick = () => {
  console.log("Clicked");
};
```

A new function reference can be created when the component renders again.

### useCallback

```jsx
const handleClick = useCallback(() => {
  console.log("Clicked");
}, []);
```

React can reuse the same function reference while dependencies remain unchanged.

---

# 11. useCallback vs useMemo

This is one of the most important interview questions.

### useMemo

Memoizes a **value**:

```jsx
const result = useMemo(() => {
  return calculate(a);
}, [a]);
```

### useCallback

Memoizes a **function**:

```jsx
const handleClick = useCallback(() => {
  console.log("Clicked");
}, []);
```

### Easy Memory

```text
useMemo
   ↓
VALUE

useCallback
   ↓
FUNCTION
```

---

# 12. useCallback vs React.memo

### React.memo

Memoizes a **component**.

```jsx
const Child = React.memo(function Child() {
  return <h1>Hello</h1>;
});
```

### useCallback

Memoizes a **function reference**.

```jsx
const handleClick = useCallback(() => {
  console.log("Clicked");
}, []);
```

### Together

```text
useCallback
    ↓
Stable function prop
    ↓
React.memo
    ↓
Child can skip unnecessary render
```

---

# 13. Example with Function Prop

```jsx
const Child = React.memo(({ onDelete }) => {
  return (
    <button onClick={onDelete}>
      Delete
    </button>
  );
});

function App() {
  const [count, setCount] = useState(0);

  const handleDelete = useCallback(() => {
    console.log("Delete");
  }, []);

  return (
    <>
      <button onClick={() => setCount(count + 1)}>
        Count: {count}
      </button>

      <Child onDelete={handleDelete} />
    </>
  );
}
```

When `count` changes:

```text
App re-renders
      ↓
handleDelete reference remains stable
      ↓
Child receives same function prop
      ↓
React.memo can skip Child render
```

---

# 14. Functional State Update with useCallback

Suppose:

```jsx
const [count, setCount] = useState(0);
```

We want:

```jsx
const increment = useCallback(() => {
  setCount(count + 1);
}, [count]);
```

This depends on `count`.

Another approach uses a functional update:

```jsx
const increment = useCallback(() => {
  setCount(prev => prev + 1);
}, []);
```

Now the callback does not need `count` as a dependency because it receives the previous state from React.

---

# 15. When Should You Use useCallback?

Useful when:

- Passing functions to memoized child components
- Function reference stability matters
- A function is used as a dependency of another Hook
- Profiling shows unnecessary work caused by changing function references

Example:

```jsx
const handleSearch = useCallback(() => {
  // search logic
}, [query]);
```

---

# 16. When Should You NOT Use useCallback?

Don't use it automatically for every function.

❌ This is usually unnecessary:

```jsx
const add = useCallback(() => {
  return 10 + 20;
}, []);
```

For a simple component with no meaningful reference-stability requirement, a normal function is usually enough.

### Important

> `useCallback` is a performance optimization, not a requirement for writing event handlers.

---

# 17. useCallback Has a Cost

`useCallback` also requires React to:

```text
Store the function
+
Track dependencies
+
Compare dependencies
```

Therefore:

```text
useCallback everywhere
        ❌
```

is not automatically better.

Use it when there is a real reason.

---

# 18. Common Mistakes

### ❌ Mistake 1: Confusing useCallback with useMemo

```text
useCallback → Function
useMemo     → Value
```

---

### ❌ Mistake 2: Calling the function inside useCallback

❌ Wrong:

```jsx
useCallback(handleClick(), []);
```

This calls the function immediately.

✅ Correct:

```jsx
useCallback(() => {
  handleClick();
}, []);
```

Or simply:

```jsx
useCallback(handleClick, []);
```

when the dependency behavior is appropriate.

---

### ❌ Mistake 3: Missing dependencies

```jsx
const handleClick = useCallback(() => {
  console.log(name);
}, []);
```

If `name` can change, this can cause the callback to use stale data.

Better:

```jsx
const handleClick = useCallback(() => {
  console.log(name);
}, [name]);
```

---

### ❌ Mistake 4: Thinking useCallback prevents re-renders

It does not.

`useCallback` only helps maintain a stable function reference.

---

# 19. Real-World Example

Suppose we have a product list:

```text
ProductList
    ↓
ProductCard
    ↓
Delete Button
```

Parent:

```jsx
const handleDelete = useCallback((id) => {
  console.log("Delete:", id);
}, []);
```

Child:

```jsx
const ProductCard = React.memo(({ product, onDelete }) => {
  return (
    <button onClick={() => onDelete(product.id)}>
      Delete
    </button>
  );
});
```

Here:

```text
useCallback
    ↓
Stable onDelete function
    ↓
React.memo
    ↓
Avoid unnecessary ProductCard renders
```

---

# 20. Interview Questions

### Q1. What is useCallback?

> `useCallback` is a React Hook that memoizes a function reference and returns the same function until its dependencies change.

### Q2. Why do we use useCallback?

To maintain function reference stability and, in appropriate cases, reduce unnecessary re-renders of memoized child components.

### Q3. Does useCallback prevent re-rendering?

❌ No.

It memoizes a function reference.

### Q4. What is the difference between useMemo and useCallback?

```text
useMemo
→ Memoizes a value

useCallback
→ Memoizes a function
```

### Q5. When is useCallback commonly used?

When passing callback functions to memoized child components or when stable function identity matters.

### Q6. Should useCallback be used for every function?

❌ No.

Use it when it provides a meaningful benefit.

---

# 21. Quick Revision

```text
useCallback
      ↓
React Hook
      ↓
Memoizes FUNCTION
      ↓
Checks dependencies
      ↓
Dependencies unchanged?
      ↓
Reuse function reference
```

### Easy Memory

```text
React.memo  → Component
useMemo     → Value
useCallback → Function
```

# 🎯 Interview One-Liner

> **useCallback memoizes a function reference so that the same function can be reused across renders until its dependencies change.**

# ⭐ Key Point

```text
useCallback does NOT make a function faster.

It mainly helps maintain a stable function reference.
```
