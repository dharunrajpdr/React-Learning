# 📘 React – Performance

## 1. What is React Performance?

**React Performance** means making a React application run smoothly and efficiently.

A good-performing React application should:

- Render UI efficiently
- Avoid unnecessary re-renders
- Load quickly
- Respond quickly to user actions
- Handle large amounts of data efficiently

---

# 2. What is a Re-render?

A **re-render** happens when React runs a component again to calculate the updated UI.

For example:

```jsx
function Counter() {
  const [count, setCount] = useState(0);

  return (
    <button onClick={() => setCount(count + 1)}>
      {count}
    </button>
  );
}
```

When:

```jsx
setCount(count + 1);
```

is called:

```text
State changes
    ↓
Component re-renders
    ↓
React calculates updated UI
    ↓
DOM is updated where necessary
```

---

# 3. Does Every Re-render Mean a Problem?

❌ No.

Re-rendering is a normal part of React.

The problem is **unnecessary re-rendering**, especially when:

- Components are large
- Lists contain many items
- Expensive calculations run repeatedly
- API/data processing is repeated unnecessarily

---

# 4. Common Causes of Performance Problems

### 1. Unnecessary re-renders

A component may render even when its output does not need to change.

### 2. Large lists

Rendering thousands of items at once can be expensive.

### 3. Expensive calculations

Example:

```jsx
const result = expensiveCalculation(data);
```

If this runs on every render, performance may suffer.

### 4. Unnecessary API calls

Repeated API calls can slow down the application.

### 5. Large images

Large image files increase loading time.

### 6. Unnecessary state updates

Updating state too frequently can cause extra renders.

---

# 5. How to Improve React Performance?

Common techniques include:

```text
React.memo
useMemo
useCallback
Code Splitting
Lazy Loading
List Virtualization
Debouncing
Throttling
Proper State Management
Image Optimization
```

---

# 6. React.memo

`React.memo` prevents a component from re-rendering when its props have not changed.

Example:

```jsx
const User = React.memo(function User({ name }) {
  console.log("User rendered");

  return <h2>{name}</h2>;
});
```

If the parent re-renders but `name` remains the same, React can skip rendering `User`.

### Simple Memory

```text
React.memo
    ↓
Memoize Component
    ↓
Avoid unnecessary re-render
```

We will study `React.memo` separately in the next topic.

---

# 7. useMemo

`useMemo` memoizes the **result of a calculation**.

Example:

```jsx
const result = useMemo(() => {
  return expensiveCalculation(number);
}, [number]);
```

React recalculates only when `number` changes.

### Simple Memory

```text
useMemo
    ↓
Memoize VALUE
```

---

# 8. useCallback

`useCallback` memoizes a **function reference**.

Example:

```jsx
const handleClick = useCallback(() => {
  console.log("Clicked");
}, []);
```

### Simple Memory

```text
useCallback
    ↓
Memoize FUNCTION
```

---

# 9. React.memo vs useMemo vs useCallback

| Feature | Purpose |
|---|---|
| `React.memo` | Memoizes a component |
| `useMemo` | Memoizes a calculated value |
| `useCallback` | Memoizes a function |

### Easy Memory

```text
React.memo  → Component
useMemo     → Value
useCallback → Function
```

---

# 10. Avoid Unnecessary State

Don't create state for values that can be calculated directly.

❌ Unnecessary:

```jsx
const [fullName, setFullName] = useState("");

useEffect(() => {
  setFullName(firstName + " " + lastName);
}, [firstName, lastName]);
```

If `fullName` can simply be calculated:

✅ Better:

```jsx
const fullName = firstName + " " + lastName;
```

This avoids an unnecessary state update and effect.

---

# 11. Keep State as Local as Possible

Suppose only one component needs a piece of state.

Instead of putting it at the top-level unnecessarily:

```text
App
 ↓
Many Components
 ↓
One Component needs state
```

Keep the state close to that component when possible.

```text
Component
   ↓
Own State
```

This can reduce unnecessary updates in unrelated parts of the application.

---

# 12. Avoid Unnecessary API Calls

❌ Don't repeatedly call an API without a reason.

For example:

```jsx
useEffect(() => {
  fetchUsers();
});
```

Without a dependency array, this can run after every render.

✅ If it should run on initial mount:

```jsx
useEffect(() => {
  fetchUsers();
}, []);
```

Always choose dependencies based on the actual logic.

---

# 13. Optimize Large Lists

Suppose we have:

```jsx
users.map(user => (
  <User key={user.id} user={user} />
))
```

If there are thousands of users, rendering everything at once can be expensive.

Techniques include:

```text
Pagination
Virtualization
Lazy loading
Memoization
```

### Pagination

Instead of:

```text
10,000 users
     ↓
Render all
```

Render:

```text
Page 1 → 20 users
Page 2 → 20 users
Page 3 → 20 users
```

---

# 14. List Virtualization

Virtualization renders only the items currently visible on the screen.

Example:

```text
10,000 items
     ↓
Only visible items rendered
     ↓
Scroll
     ↓
New visible items rendered
```

This is useful for:

- Large tables
- Chat messages
- Long lists
- Large datasets

Libraries such as `react-window` can be used for this purpose.

---

# 15. Code Splitting

Code splitting means splitting a large JavaScript bundle into smaller pieces.

Instead of loading everything:

```text
Application
    ↓
One huge bundle
```

We can load code when needed:

```text
Application
   ↓
Small initial bundle
   ↓
Load additional code when required
```

This can improve initial loading performance.

---

# 16. Lazy Loading

React provides `lazy()` for loading components only when needed.

Example:

```jsx
import { lazy, Suspense } from "react";

const Dashboard = lazy(() => import("./Dashboard"));
```

Use it with:

```jsx
<Suspense fallback={<p>Loading...</p>}>
  <Dashboard />
</Suspense>
```

### Flow

```text
User needs Dashboard
       ↓
Dashboard component loads
       ↓
Show component
```

---

# 17. Debouncing

**Debouncing** delays a function until the user stops performing an action for a certain amount of time.

Useful for:

- Search boxes
- API search
- Autocomplete

Example situation:

```text
User types:

R
Re
Rea
React
```

Instead of making 5 API calls:

```text
R     → API
Re    → API
Rea   → API
Reac  → API
React → API
```

Debouncing can wait until the user stops typing:

```text
React
  ↓
Wait
  ↓
API call
```

---

# 18. Throttling

**Throttling** limits how frequently a function can execute.

Useful for:

- Scroll events
- Mouse movement
- Resize events

Example:

```text
User scrolls continuously
       ↓
Function executes
       ↓
Wait for interval
       ↓
Function executes again
```

### Difference

```text
Debounce
→ Execute after activity stops

Throttle
→ Execute at controlled intervals
```

---

# 19. Optimize Images

Large images can increase page loading time.

Good practices:

```text
Use appropriate image sizes
Compress images
Use modern formats such as WebP/AVIF when appropriate
Lazy-load images when suitable
```

Example:

```jsx
<img
  src="/profile.webp"
  alt="Profile"
  loading="lazy"
/>
```

---

# 20. Avoid Unnecessary Object Creation

Consider:

```jsx
<Child user={{ name: "Dharun" }} />
```

A new object is created on each parent render.

For memoized child components, changing object references can matter.

Depending on the situation, `useMemo` can preserve the object reference:

```jsx
const user = useMemo(() => ({
  name: "Dharun"
}), []);
```

Then:

```jsx
<Child user={user} />
```

### Important

Don't use `useMemo` everywhere.

Use it when reference stability or expensive calculation actually matters.

---

# 21. Avoid Unnecessary Function Creation

Example:

```jsx
<Child onClick={() => handleClick()} />
```

A new function is created during each render.

When passing callbacks to memoized children, `useCallback` can sometimes help:

```jsx
const handleClick = useCallback(() => {
  console.log("Clicked");
}, []);
```

Then:

```jsx
<Child onClick={handleClick} />
```

Again, don't use `useCallback` everywhere.

---

# 22. React DevTools Profiler

React DevTools provides a **Profiler** that helps identify components that render and how much time rendering takes.

It can help answer:

```text
Which component rendered?
Why did it render?
How long did rendering take?
```

Use profiling to find actual bottlenecks instead of optimizing blindly.

---

# 23. Performance Optimization Flow

```text
Application feels slow
        ↓
Measure / Profile
        ↓
Find bottleneck
        ↓
Identify unnecessary work
        ↓
Optimize specific problem
        ↓
Measure again
```

### Important

**Measure first → Optimize second.**

---

# 24. Common Performance Mistakes

### ❌ Using `useMemo` everywhere

Not every calculation needs memoization.

### ❌ Using `useCallback` everywhere

It also has a cost and is mainly useful when function identity matters.

### ❌ Keeping all state globally

Keep state local when possible.

### ❌ Rendering huge lists at once

Use pagination or virtualization when appropriate.

### ❌ Making unnecessary API calls

Use proper effects, caching, or request management.

### ❌ Optimizing without measuring

First identify the actual bottleneck.

---

# 25. Interview Questions

### Q1. What is React performance?

It is the practice of making React applications render and respond efficiently while avoiding unnecessary work.

### Q2. What is unnecessary re-rendering?

When a component renders even though the relevant UI did not need to change.

### Q3. How can you improve React performance?

Common techniques include:

```text
React.memo
useMemo
useCallback
Lazy loading
Code splitting
Pagination
Virtualization
Debouncing
Throttling
State optimization
Image optimization
```

### Q4. What is code splitting?

Code splitting divides application code into smaller chunks that can be loaded when needed.

### Q5. What is lazy loading?

Lazy loading means loading a resource or component only when it is needed.

### Q6. What is the difference between debounce and throttle?

> **Debounce** runs after activity stops, while **throttle** limits execution to a controlled frequency.

---

# 26. Quick Revision

```text
React Performance
       ↓
Avoid unnecessary work
       ↓
Measure first
       ↓
Optimize bottlenecks
```

### Important tools

```text
React.memo  → Component
useMemo     → Value
useCallback → Function
```

### Large data

```text
Pagination
Virtualization
Lazy Loading
```

### User actions

```text
Debounce
Throttle
```

### Main Rule

> **Don't optimize everything. Measure first and optimize the actual bottleneck.**

---

# 🎯 Interview One-Liner

> React performance optimization means reducing unnecessary rendering and expensive work so the application remains fast and responsive.

# ⭐ Key Point

```text
Good React Performance
        ↓
Less unnecessary work
        ↓
Faster UI
        ↓
Better user experience
```
