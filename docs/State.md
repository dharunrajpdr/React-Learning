# 📘 React – State

## 1. What is State?

**State** is data that is **managed inside a React component** and can change over time.

When state changes, React **re-renders the component** to update the UI.

### Simple Definition

> State is a component's internal data that can change and cause the UI to update.

---

# 2. Why Do We Need State?

Consider a counter:

```text
Count = 0
     ↓
User clicks button
     ↓
Count = 1
     ↓
UI should display 1
```

We need state to store the changing value.

---

# 3. Basic State Example

React provides the `useState` Hook to create state.

```jsx
import { useState } from "react";

function Counter() {

    const [count, setCount] = useState(0);

    return (
        <div>
            <h2>Count: {count}</h2>

            <button onClick={() => setCount(count + 1)}>
                Increase
            </button>
        </div>
    );
}

export default Counter;
```

### Output

Initially:

```text
Count: 0
[Increase]
```

After clicking:

```text
Count: 1
[Increase]
```

---

# 4. Understanding `useState`

This line:

```jsx
const [count, setCount] = useState(0);
```

contains three important parts:

```text
count
  ↓
Current state value

setCount
  ↓
Function used to update state

0
  ↓
Initial value
```

### General Syntax

```jsx
const [state, setState] = useState(initialValue);
```

---

# 5. Updating State

We update state using its setter function.

```jsx
setCount(10);
```

For example:

```jsx
const [name, setName] = useState("Dharun");

setName("Ajay");
```

Now:

```text
name = "Ajay"
```

---

# 6. State Causes Re-render

When state changes:

```text
setCount(...)
      ↓
State changes
      ↓
Component re-renders
      ↓
Updated UI
```

Example:

```jsx
setCount(count + 1);
```

React updates the component with the new value.

---

# 7. State Can Store Different Data Types

State can store:

### String

```jsx
const [name, setName] = useState("Dharun");
```

### Number

```jsx
const [age, setAge] = useState(21);
```

### Boolean

```jsx
const [isLoggedIn, setIsLoggedIn] = useState(false);
```

### Array

```jsx
const [skills, setSkills] = useState([]);
```

### Object

```jsx
const [student, setStudent] = useState({
    name: "Dharun",
    age: 21
});
```

---

# 8. State with Input

State is commonly used to control form inputs.

```jsx
import { useState } from "react";

function App() {

    const [name, setName] = useState("");

    return (
        <div>

            <input
                value={name}
                onChange={(e) => setName(e.target.value)}
            />

            <h2>Hello {name}</h2>

        </div>
    );
}
```

If the user types:

```text
Dharun
```

The UI displays:

```text
Hello Dharun
```

---

# 9. State vs Normal Variable

### Normal Variable

```jsx
function App() {

    let count = 0;

    function increase() {
        count++;
    }

    return <h2>{count}</h2>;
}
```

Changing `count` does not tell React to update the UI.

### State

```jsx
const [count, setCount] = useState(0);

setCount(count + 1);
```

React knows the state changed and re-renders the component.

---

# 10. Props vs State

| Props | State |
|---|---|
| Passed from parent | Managed inside component |
| Read-only | Can be updated |
| Used to receive data | Used to manage changing data |
| Parent controls the value | Component manages the value |
| Parent → Child | Internal component data |

### Easy Memory

```text
Props → Received
State → Managed
```

---

# 11. State Should Not Be Modified Directly

### ❌ Wrong

```jsx
count = count + 1;
```

### ✅ Correct

```jsx
setCount(count + 1);
```

Always use the state setter to update state.

---

# 12. Functional State Update

When the new state depends on the previous state, we can use a function.

```jsx
setCount(prevCount => prevCount + 1);
```

Example:

```jsx
function Counter() {

    const [count, setCount] = useState(0);

    function increase() {
        setCount(prevCount => prevCount + 1);
    }

    return (
        <button onClick={increase}>
            {count}
        </button>
    );
}
```

This is useful when multiple updates depend on the previous state.

---

# 13. Multiple State Variables

A component can have multiple states.

```jsx
function Student() {

    const [name, setName] = useState("Dharun");
    const [age, setAge] = useState(21);
    const [isStudent, setIsStudent] = useState(true);

    return (
        <div>
            <h2>{name}</h2>
            <p>{age}</p>
            <p>{isStudent ? "Student" : "Not Student"}</p>
        </div>
    );
}
```

---

# 14. State with Array

Example:

```jsx
const [skills, setSkills] = useState([]);
```

Add a skill:

```jsx
setSkills([...skills, "React"]);
```

Now:

```text
["React"]
```

Add another:

```jsx
setSkills([...skills, "Java"]);
```

Now:

```text
["React", "Java"]
```

---

# 15. State with Object

Example:

```jsx
const [student, setStudent] = useState({
    name: "Dharun",
    age: 21
});
```

Update the name:

```jsx
setStudent({
    ...student,
    name: "Ajay"
});
```

The spread operator keeps the other properties.

---

# 16. Important Rules of State

```text
1. Don't modify state directly.
2. Use the setter function.
3. State changes can cause re-rendering.
4. State is local to a component unless shared through other mechanisms.
5. Use functional updates when the new value depends on the previous value.
```

---

# 🧠 Easy Memory Trick

```text
State
 ↓
Component Data
 ↓
Can Change
 ↓
setState()
 ↓
Re-render
 ↓
Updated UI
```

---

# 🎯 Interview One-Liners

### What is state?

> State is data managed inside a React component that can change over time and cause the component to re-render.

### What is `useState`?

> `useState` is a React Hook used to add state to a functional component.

### How do you update state?

> We update state using the setter function returned by `useState`.

### Can we modify state directly?

> No. We should use the state setter function instead of modifying state directly.

### What happens when state changes?

> React schedules a re-render so the UI can reflect the updated state.

### Props vs State?

> Props are read-only data received from a parent, while state is internal data managed by a component.

---

# 🔥 Quick Revision

```text
State
→ Internal component data
→ Can change
→ useState() creates state
→ Setter updates state
→ State change causes re-render
→ Don't modify state directly
→ Can store string, number, boolean, array, object, etc.
```

### Basic Syntax

```jsx
const [state, setState] = useState(initialValue);
```

### Example

```jsx
const [count, setCount] = useState(0);

setCount(count + 1);
```

# ⭐ Key Point

> **State stores changing data inside a component, and updating state allows React to re-render the UI with the new data.**
