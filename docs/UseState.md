# 📘 React – useState Hook

## 1. What is `useState`?

`useState` is a React Hook used to create and manage **state** inside a functional component.

### Simple Definition

> `useState` allows a component to store data and update the UI when that data changes.

---

# 2. Importing `useState`

```jsx
import { useState } from "react";
```

---

# 3. Basic Syntax

```jsx
const [state, setState] = useState(initialValue);
```

Example:

```jsx
const [count, setCount] = useState(0);
```

Here:

```text
count
→ Current state value

setCount
→ Function used to update state

0
→ Initial value
```

---

# 4. Simple Counter Example

```jsx
import { useState } from "react";

function App() {

    const [count, setCount] = useState(0);

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

### Flow

```text
Click button
     ↓
setCount(count + 1)
     ↓
State changes
     ↓
Component re-renders
     ↓
New value displayed
```

---

# 5. Updating State

Never directly modify the state.

### ❌ Wrong

```jsx
count = count + 1;
```

### ❌ Wrong

```jsx
count++;
```

### ✅ Correct

```jsx
setCount(count + 1);
```

Always use the state setter.

---

# 6. State with String

```jsx
import { useState } from "react";

function App() {

    const [name, setName] = useState("");

    return (
        <div>

            <h2>Hello {name}</h2>

            <button onClick={() => setName("Dharun")}>
                Set Name
            </button>

        </div>
    );
}
```

---

# 7. State with Boolean

Boolean state is useful for things like:

```text
Login
Logout
Show
Hide
Dark mode
Menu open/close
```

Example:

```jsx
import { useState } from "react";

function App() {

    const [isVisible, setIsVisible] = useState(true);

    return (
        <div>

            {isVisible && <h2>Hello React</h2>}

            <button onClick={() => setIsVisible(!isVisible)}>
                Show / Hide
            </button>

        </div>
    );
}
```

---

# 8. State with Array

```jsx
import { useState } from "react";

function App() {

    const [skills, setSkills] = useState([
        "Java",
        "JavaScript"
    ]);

    function addSkill() {

        setSkills([
            ...skills,
            "React"
        ]);
    }

    return (
        <div>

            {skills.map((skill) => (
                <p key={skill}>{skill}</p>
            ))}

            <button onClick={addSkill}>
                Add React
            </button>

        </div>
    );
}
```

### Important

Use the spread operator:

```jsx
...skills
```

to preserve the existing array.

---

# 9. State with Object

```jsx
import { useState } from "react";

function App() {

    const [student, setStudent] = useState({
        name: "Dharun",
        age: 21
    });

    function changeName() {

        setStudent({
            ...student,
            name: "Ajay"
        });
    }

    return (
        <div>

            <h2>{student.name}</h2>
            <p>{student.age}</p>

            <button onClick={changeName}>
                Change Name
            </button>

        </div>
    );
}
```

### Important

```jsx
...student
```

keeps the existing properties.

---

# 10. Functional State Update

When the new state depends on the previous state, use the functional form.

```jsx
setCount(prevCount => prevCount + 1);
```

Example:

```jsx
function increase() {
    setCount(prevCount => prevCount + 1);
}
```

### Why?

It guarantees that the update uses the latest previous state value.

---

# 11. Multiple State Variables

A component can have multiple states.

```jsx
function App() {

    const [name, setName] = useState("");
    const [age, setAge] = useState(0);
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

# 12. State and Re-rendering

When state changes:

```text
setState()
    ↓
State updated
    ↓
Component re-renders
    ↓
Updated UI
```

Example:

```jsx
setCount(count + 1);
```

React updates the component and displays the new value.

---

# 13. State Updates May Be Batched

React may group multiple state updates together.

Example:

```jsx
setCount(count + 1);
setCount(count + 1);
```

Both updates may use the same current `count` value.

When you need multiple updates based on the previous state, use:

```jsx
setCount(prev => prev + 1);
setCount(prev => prev + 1);
```

This correctly applies both updates.

---

# 14. Initial State

The value passed to `useState()` becomes the initial state.

```jsx
const [count, setCount] = useState(0);
```

Initial value:

```text
0
```

Another example:

```jsx
const [name, setName] = useState("Dharun");
```

Initial value:

```text
Dharun
```

---

# 15. Lazy Initial State

For expensive initial calculations, a function can be passed to `useState`.

```jsx
const [value, setValue] = useState(() => calculateValue());
```

The function is used to calculate the initial value.

For simple values, use:

```jsx
useState(0);
```

---

# 16. Rules of `useState`

### Rule 1: Import it

```jsx
import { useState } from "react";
```

### Rule 2: Call it at the top level of the component

```jsx
function App() {

    const [count, setCount] = useState(0);

    return <h1>{count}</h1>;
}
```

### Rule 3: Don't call it inside conditions

❌ Avoid:

```jsx
if (isLoggedIn) {
    const [name, setName] = useState("");
}
```

### Rule 4: Don't call it inside loops

❌ Avoid:

```jsx
for (...) {
    const [count, setCount] = useState(0);
}
```

Hooks should be called consistently at the top level.

---

# 17. `useState` vs Normal Variable

### Normal Variable

```jsx
let count = 0;

count++;
```

Changing it does not tell React to update the UI.

### State

```jsx
const [count, setCount] = useState(0);

setCount(count + 1);
```

React knows the state changed and can re-render the component.

---

# 18. `useState` vs Props

| `useState` | Props |
|---|---|
| Manages component state | Passes data between components |
| Can be updated by setter | Read-only from child |
| Internal data | Usually received from parent |
| `setState()` updates it | Parent controls the value |

---

# 🧠 Easy Memory Trick

```text
useState
   ↓
Store data
   ↓
setState
   ↓
Update data
   ↓
Re-render
   ↓
Updated UI
```

Remember:

```jsx
const [value, setValue] = useState(initialValue);
```

---

# 🎯 Interview One-Liners

### What is `useState`?

> `useState` is a React Hook used to add and manage state in functional components.

### What does `useState` return?

> It returns an array containing the current state value and a function to update it.

```jsx
const [count, setCount] = useState(0);
```

### Why do we use the setter function?

> The setter updates the state and tells React that the component may need to re-render.

### Can we directly modify state?

> No. We should use the state setter function instead of directly modifying the state.

### When should we use a functional state update?

> When the new state depends on the previous state.

```jsx
setCount(prev => prev + 1);
```

### Can a component have multiple `useState` calls?

> Yes, a component can have multiple state variables.

---

# 🔥 Quick Revision

```text
useState
→ Manage component state

Syntax:
const [state, setState] = useState(initialValue);

setState()
→ Updates state

State update
→ Can trigger re-render

Types:
String
Number
Boolean
Array
Object

Previous state:
setCount(prev => prev + 1)

Never:
count++

Always:
setCount(...)
```

# ⭐ Key Point

> **`useState` is the most commonly used React Hook for storing and updating component data that affects the UI.**
