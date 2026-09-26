# 📘 React – useMemo

## 1. What is useMemo?

`useMemo` is a React Hook used to **memoize a calculated value**.

It helps avoid repeating an expensive calculation when the required dependencies have not changed.

### Simple Definition

> `useMemo` remembers the result of a calculation and recalculates it only when its dependencies change.

---

# 2. Basic Syntax

```jsx
const result = useMemo(() => {
  return calculation();
}, [dependencies]);
```

### Structure

```text
useMemo(
    function,
    dependencies
)
```

---

# 3. Simple Example

```jsx
import { useMemo, useState } from "react";

function App() {
  const [count, setCount] = useState(0);

  const square = useMemo(() => {
    return count * count;
  }, [count]);

  return (
    <>
      <h1>Count: {count}</h1>
      <h2>Square: {square}</h2>

      <button onClick={() => setCount(count + 1)}>
        Increase
      </button>
    </>
  );
}

export default App;
```

Here:

```jsx
const square = useMemo(() => {
  return count * count;
}, [count]);
```

The calculation runs when `count` changes.

---

# 4. Why Do We Need useMemo?

Consider an expensive calculation:

```jsx
const result = expensiveCalculation(data);
```

If the component re-renders many times, the calculation may run again.

With `useMemo`:

```jsx
const result = useMemo(() => {
  return expensiveCalculation(data);
}, [data]);
```

Now React can reuse the previous result when `data` has not changed.

---

# 5. How useMemo Works

```text
Component renders
       ↓
useMemo checks dependencies
       ↓
Did dependency change?
     /       \
   Yes        No
   ↓           ↓
Calculate    Use previous value
```

---

# 6. Example with Expensive Calculation

```jsx
function expensiveCalculation(num) {
  console.log("Calculation running...");

  let result = 0;

  for (let i = 0; i < 100000000; i++) {
    result += num;
  }

  return result;
}
```

Using `useMemo`:

```jsx
const result = useMemo(() => {
  return expensiveCalculation(number);
}, [number]);
```

If another unrelated state changes:

```jsx
setName("Dharun");
```

the expensive calculation does not need to run again as long as `number` remains unchanged.

---

# 7. useMemo with Multiple Dependencies

```jsx
const total = useMemo(() => {
  return price * quantity;
}, [price, quantity]);
```

The calculation runs again when:

```text
price changes
OR
quantity changes
```

If neither changes, the previous result can be reused.

---

# 8. useMemo with Arrays

`useMemo` can be useful when creating a derived array.

Example:

```jsx
const filteredUsers = useMemo(() => {
  return users.filter(user =>
    user.name.toLowerCase().includes(search.toLowerCase())
  );
}, [users, search]);
```

Here:

```text
users changes
       OR
search changes
       ↓
Filter runs again
```

Otherwise, the previous filtered array can be reused.

---

# 9. useMemo with Objects

Objects have reference identity.

Example:

```jsx
const user = useMemo(() => {
  return {
    name: "Dharun",
    age: 21
  };
}, []);
```

The same object reference can be reused across renders.

This can be useful when passing an object to a memoized child:

```jsx
<Child user={user} />
```

---

# 10. useMemo vs Normal Calculation

### Without useMemo

```jsx
const result = expensiveCalculation(data);
```

The calculation happens whenever the component renders.

### With useMemo

```jsx
const result = useMemo(() => {
  return expensiveCalculation(data);
}, [data]);
```

The previous result can be reused when `data` hasn't changed.

---

# 11. useMemo vs useCallback

This is very important for interviews.

### useMemo

Memoizes a **value**.

```jsx
const result = useMemo(() => {
  return calculate();
}, [data]);
```

### useCallback

Memoizes a **function**.

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

# 12. useMemo vs React.memo

### React.memo

Memoizes a **component** based on its props.

```jsx
const Child = React.memo(function Child({ name }) {
  return <h1>{name}</h1>;
});
```

### useMemo

Memoizes a **calculated value**.

```jsx
const result = useMemo(() => {
  return calculate(data);
}, [data]);
```

### Remember

```text
React.memo → Component
useMemo    → Value
useCallback → Function
```

---

# 13. Example: Search Filter

Suppose we have:

```jsx
const users = [
  { id: 1, name: "Dharun" },
  { id: 2, name: "Ajay" },
  { id: 3, name: "Harish" }
];
```

We want to search users.

```jsx
const filteredUsers = useMemo(() => {
  return users.filter(user =>
    user.name
      .toLowerCase()
      .includes(search.toLowerCase())
  );
}, [users, search]);
```

Then:

```jsx
return (
  <>
    <input
      value={search}
      onChange={(e) => setSearch(e.target.value)}
    />

    {filteredUsers.map(user => (
      <p key={user.id}>{user.name}</p>
    ))}
  </>
);
```

This is a common real-world use case.

---

# 14. Important: useMemo is NOT for Every Calculation

❌ Don't do this unnecessarily:

```jsx
const sum = useMemo(() => {
  return a + b;
}, [a, b]);
```

For a simple calculation like:

```jsx
const sum = a + b;
```

is usually enough.

`useMemo` is mainly useful when:

- Calculation is expensive
- Value is expensive to create
- Stable reference is useful
- Profiling shows repeated work is a problem

---

# 15. useMemo Has a Cost

`useMemo` itself requires React to:

```text
Store the value
+
Track dependencies
+
Check dependencies
```

Therefore:

> Memoization is not automatically faster.

Use it when there is a meaningful performance benefit.

---

# 16. Dependency Array

Example:

```jsx
const result = useMemo(() => {
  return calculate(a, b);
}, [a, b]);
```

Dependencies are:

```text
a
b
```

### If `a` changes:

```text
Recalculate
```

### If `b` changes:

```text
Recalculate
```

### If neither changes:

```text
Reuse previous result
```

---

# 17. Empty Dependency Array

Example:

```jsx
const value = useMemo(() => {
  return expensiveCalculation();
}, []);
```

The calculation is intended to be reused across subsequent renders because there are no dependencies.

However, don't use `[]` blindly. Every value used by the calculation that can change should be considered when determining dependencies.

---

# 18. Common Mistakes

### ❌ Mistake 1: Using useMemo everywhere

```jsx
useMemo(() => a + b, [a, b]);
```

for every small calculation is unnecessary.

---

### ❌ Mistake 2: Missing dependencies

```jsx
const result = useMemo(() => {
  return price * quantity;
}, [price]);
```

`quantity` is also used, so omitting it can produce stale results.

Better:

```jsx
const result = useMemo(() => {
  return price * quantity;
}, [price, quantity]);
```

---

### ❌ Mistake 3: Thinking useMemo prevents re-renders

`useMemo` does **not** prevent the component from rendering.

It memoizes a value **inside the render**.

---

### ❌ Mistake 4: Confusing useMemo with useCallback

```text
useMemo     → value
useCallback → function
```

---

# 19. When Should You Use useMemo?

Use it when:

```text
Expensive calculation
        OR
Expensive derived value
        OR
Stable object/array reference is useful
        OR
Profiling identifies repeated expensive work
```

Example:

```jsx
const sortedUsers = useMemo(() => {
  return [...users].sort((a, b) =>
    a.name.localeCompare(b.name)
  );
}, [users]);
```

---

# 20. When Should You Avoid useMemo?

Avoid unnecessary use when:

- Calculation is very simple
- Component is already fast
- Value changes on every render anyway
- There is no measurable benefit

---

# 21. Interview Questions

### Q1. What is useMemo?

> `useMemo` is a React Hook that memoizes a calculated value and recomputes it when its dependencies change.

### Q2. Why is useMemo used?

To avoid unnecessary expensive calculations and sometimes preserve stable object/array references.

### Q3. Does useMemo prevent re-rendering?

❌ No.

It only memoizes a value.

### Q4. What does useMemo return?

It returns the memoized value.

```jsx
const value = useMemo(() => {
  return calculation();
}, []);
```

### Q5. What is the difference between useMemo and useCallback?

```text
useMemo
→ Memoizes a value

useCallback
→ Memoizes a function
```

### Q6. Should useMemo be used everywhere?

❌ No.

It should be used when it provides a meaningful performance benefit.

---

# 22. Quick Revision

```text
useMemo
   ↓
React Hook
   ↓
Memoizes a VALUE
   ↓
Checks dependencies
   ↓
Dependency changed?
   ↓
Yes → Recalculate
No  → Reuse previous value
```

### Easy Memory

```text
React.memo  → Component
useMemo     → Value
useCallback → Function
```

# 🎯 Interview One-Liner

> **useMemo is used to memoize a calculated value so that expensive calculations do not unnecessarily run again when their dependencies have not changed.**

# ⭐ Key Point

```text
useMemo optimizes a VALUE.
It does not stop the component from re-rendering.
```
