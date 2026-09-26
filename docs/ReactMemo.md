# 📘 React – React.memo

## 1. What is React.memo?

`React.memo` is a **higher-order component (HOC)** used to prevent unnecessary re-renders of a functional component.

It tells React:

> "If the component's props have not changed, reuse the previous rendered result."

### Simple Definition

```text
React.memo
    ↓
Memoizes a component
    ↓
Avoids unnecessary re-rendering
```

---

# 2. Basic Syntax

```jsx
const MyComponent = React.memo(function MyComponent(props) {
  return <h1>{props.name}</h1>;
});
```

Or:

```jsx
const MyComponent = React.memo(({ name }) => {
  return <h1>{name}</h1>;
});
```

---

# 3. Simple Example

### Without React.memo

```jsx
function Child({ name }) {
  console.log("Child rendered");

  return <h2>Hello {name}</h2>;
}

function App() {
  const [count, setCount] = useState(0);

  return (
    <>
      <button onClick={() => setCount(count + 1)}>
        Count: {count}
      </button>

      <Child name="Dharun" />
    </>
  );
}
```

When `count` changes, `App` re-renders.

The child may also render again even though:

```text
name = "Dharun"
```

has not changed.

---

# 4. Using React.memo

```jsx
const Child = React.memo(function Child({ name }) {
  console.log("Child rendered");

  return <h2>Hello {name}</h2>;
});
```

Now:

```text
Parent re-renders
      ↓
React checks Child props
      ↓
Props unchanged?
      ↓
Yes
      ↓
Child render can be skipped
```

---

# 5. Complete Example

```jsx
import { useState } from "react";

const Child = React.memo(function Child({ name }) {
  console.log("Child rendered");

  return <h2>Hello {name}</h2>;
});

function App() {
  const [count, setCount] = useState(0);

  return (
    <>
      <button onClick={() => setCount(prev => prev + 1)}>
        Count: {count}
      </button>

      <Child name="Dharun" />
    </>
  );
}

export default App;
```

When the button is clicked:

```text
App re-renders
      ↓
Child receives same name
      ↓
Child can skip re-render
```

---

# 6. What Does React.memo Compare?

By default, `React.memo` compares the component's props using a **shallow comparison**.

For primitive values:

```jsx
<Child name="Dharun" />
```

If the value remains the same:

```text
"Dharun" === "Dharun"
```

the prop is considered unchanged.

---

# 7. React.memo with Numbers

```jsx
const Student = React.memo(({ age }) => {
  console.log("Student rendered");

  return <h2>Age: {age}</h2>;
});
```

Usage:

```jsx
<Student age={21} />
```

If the parent re-renders but `age` remains `21`, the child can skip the render.

---

# 8. React.memo with Objects

Be careful with objects.

Example:

```jsx
<Child user={{ name: "Dharun" }} />
```

A new object is created during each parent render.

Even though the content looks the same:

```js
{ name: "Dharun" }
```

the object reference can be different.

Therefore:

```text
Object 1 !== Object 2
```

from a reference comparison perspective.

This can cause a memoized child to render again.

---

# 9. Using useMemo with Objects

If appropriate, we can preserve the object reference:

```jsx
const user = useMemo(() => ({
  name: "Dharun"
}), []);
```

Then:

```jsx
<Child user={user} />
```

Now the same object reference can be passed across renders.

---

# 10. React.memo with Functions

Similar issue can happen with functions.

Example:

```jsx
<Child onClick={() => console.log("Clicked")} />
```

A new function is created during each render.

For a memoized child, `useCallback` can preserve the function reference when appropriate:

```jsx
const handleClick = useCallback(() => {
  console.log("Clicked");
}, []);
```

Then:

```jsx
<Child onClick={handleClick} />
```

---

# 11. React.memo + useCallback

These are often used together when a memoized child receives callback props.

```jsx
const Child = React.memo(({ onClick }) => {
  return (
    <button onClick={onClick}>
      Click
    </button>
  );
});

function App() {
  const handleClick = useCallback(() => {
    console.log("Clicked");
  }, []);

  return <Child onClick={handleClick} />;
}
```

### Flow

```text
Parent
  ↓
useCallback()
  ↓
Stable function reference
  ↓
React.memo Child
  ↓
Child can skip unnecessary render
```

---

# 12. React.memo Does NOT Stop Every Re-render

`React.memo` mainly helps when a component receives the **same props**.

A memoized component can still render when its own state changes.

Example:

```jsx
const Child = React.memo(() => {
  const [count, setCount] = useState(0);

  return (
    <button onClick={() => setCount(count + 1)}>
      {count}
    </button>
  );
});
```

When its own state changes:

```text
Child state changes
      ↓
Child re-renders
```

`React.memo` does not prevent this.

---

# 13. React.memo and Context

If a component uses Context:

```jsx
const theme = useContext(ThemeContext);
```

and the relevant context value changes, the component can re-render even if its props remain the same.

So:

```text
React.memo
    ↓
Checks props
```

It does not make a component completely immune to other sources of updates.

---

# 14. When Should We Use React.memo?

Useful when:

- Component renders frequently
- Component is relatively expensive to render
- Parent re-renders frequently
- Props often remain unchanged
- Profiling shows unnecessary renders

Example:

```text
Large Product List
       ↓
ProductCard
       ↓
React.memo
```

If most product props don't change, unnecessary renders can be reduced.

---

# 15. When Should We NOT Use React.memo?

Don't automatically wrap every component:

```jsx
const Component = React.memo(...)
```

Memoization has its own overhead.

It may provide little benefit when:

- Component is very small
- Component rarely re-renders
- Props change frequently
- Rendering is already cheap

### Important

> Use `React.memo` when it solves an actual performance problem.

---

# 16. React.memo vs useMemo vs useCallback

| Feature | React.memo | useMemo | useCallback |
|---|---|---|---|
| Memoizes | Component | Value | Function |
| Main purpose | Avoid component re-render | Avoid repeated calculation | Preserve function reference |
| Used as | HOC | Hook | Hook |
| Example | `React.memo(Child)` | `useMemo(...)` | `useCallback(...)` |

### Easy Memory

```text
React.memo  → Component
useMemo     → Value
useCallback → Function
```

---

# 17. React.memo vs useMemo

### React.memo

```jsx
const Child = React.memo(({ name }) => {
  return <h1>{name}</h1>;
});
```

Memoizes the **component rendering based on props**.

### useMemo

```jsx
const result = useMemo(() => {
  return expensiveCalculation(data);
}, [data]);
```

Memoizes the **calculated value**.

---

# 18. Custom Comparison Function

`React.memo` can optionally receive a comparison function.

```jsx
const Child = React.memo(
  function Child({ user }) {
    return <h2>{user.name}</h2>;
  },
  (prevProps, nextProps) => {
    return prevProps.user.name === nextProps.user.name;
  }
);
```

If the comparison function returns:

```text
true
```

React can skip the re-render.

If it returns:

```text
false
```

React renders the component.

### Important

Use custom comparison carefully because an expensive comparison can itself hurt performance.

---

# 19. Common Mistakes

### ❌ Mistake 1: Using React.memo everywhere

```jsx
React.memo(Everything)
```

is not automatically better.

---

### ❌ Mistake 2: Passing a new object every render

```jsx
<Child user={{ name: "Dharun" }} />
```

---

### ❌ Mistake 3: Passing a new function every render

```jsx
<Child onClick={() => handleClick()} />
```

For memoized children, consider `useCallback` when useful.

---

### ❌ Mistake 4: Thinking React.memo prevents all renders

It primarily optimizes based on props.

State and context changes can still cause renders.

---

# 20. Interview Questions

### Q1. What is React.memo?

> `React.memo` is a higher-order component that memoizes a functional component and can skip re-rendering when its props have not changed.

### Q2. Why is React.memo used?

To reduce unnecessary re-renders and improve performance when a component frequently receives the same props.

### Q3. Does React.memo prevent all re-renders?

No.

The component can still re-render because of its own state or relevant context changes.

### Q4. What comparison does React.memo use?

By default, it uses shallow comparison of props.

### Q5. Can React.memo work with functions?

Yes, but if a function prop is recreated on every parent render, `useCallback` may be useful to preserve its reference when appropriate.

### Q6. What is the difference between React.memo and useMemo?

```text
React.memo → memoizes a component
useMemo    → memoizes a calculated value
```

---

# 21. Quick Revision

```text
React.memo
    ↓
Memoize Component
    ↓
Compare Props
    ↓
Props unchanged?
    ↓
Yes → Can skip re-render
No  → Re-render
```

### Remember

```text
React.memo  → Component
useMemo     → Value
useCallback → Function
```

# 🎯 Interview One-Liner

> **React.memo is used to prevent unnecessary re-renders of a functional component when its props have not changed.**

# ⭐ Key Point

```text
React.memo is a performance optimization,
not something that every component needs.
```
