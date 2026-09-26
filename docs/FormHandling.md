# 📘 React – Forms & Form Handling

## 1. What is a Form?

A **form** is used to collect user input.

Examples:

```text
Login Form
Registration Form
Contact Form
Search Form
Payment Form
```

In React, forms are usually handled using **state** and event handlers.

### Simple Definition

> React form handling is the process of managing form inputs, validation, and submission using React.

---

# 2. Basic Form

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

# 3. Why `preventDefault()`?

Normally, submitting a form can reload or navigate the page.

In React:

```jsx
event.preventDefault();
```

prevents the browser's default form submission behavior.

```jsx
function handleSubmit(event) {

    event.preventDefault();

    console.log("Form submitted");
}
```

---

# 4. Handling Input with State

We can store the input value in state.

```jsx
import { useState } from "react";

function App() {

    const [name, setName] = useState("");

    return (
        <input
            value={name}
            onChange={(event) => setName(event.target.value)}
        />
    );
}
```

### Flow

```text
User types
    ↓
onChange
    ↓
event.target.value
    ↓
setName()
    ↓
State updated
```

---

# 5. Complete Form Example

```jsx
import { useState } from "react";

function App() {

    const [name, setName] = useState("");

    function handleSubmit(event) {

        event.preventDefault();

        console.log(name);
    }

    return (
        <form onSubmit={handleSubmit}>

            <input
                type="text"
                value={name}
                onChange={(event) => setName(event.target.value)}
            />

            <button type="submit">
                Submit
            </button>

        </form>
    );
}
```

If the user enters:

```text
Dharun
```

Then:

```js
name
```

contains:

```text
Dharun
```

---

# 6. Multiple Input Fields

Suppose we have:

```text
Name
Email
Password
```

We can use separate states:

```jsx
import { useState } from "react";

function App() {

    const [name, setName] = useState("");
    const [email, setEmail] = useState("");
    const [password, setPassword] = useState("");

    function handleSubmit(event) {

        event.preventDefault();

        console.log(name);
        console.log(email);
        console.log(password);
    }

    return (
        <form onSubmit={handleSubmit}>

            <input
                type="text"
                placeholder="Name"
                value={name}
                onChange={(e) => setName(e.target.value)}
            />

            <input
                type="email"
                placeholder="Email"
                value={email}
                onChange={(e) => setEmail(e.target.value)}
            />

            <input
                type="password"
                placeholder="Password"
                value={password}
                onChange={(e) => setPassword(e.target.value)}
            />

            <button type="submit">
                Register
            </button>

        </form>
    );
}
```

---

# 7. Using One State Object

Instead of multiple states, we can store form data in one object.

```jsx
import { useState } from "react";

function App() {

    const [form, setForm] = useState({
        name: "",
        email: "",
        password: ""
    });

    return (
        <div>
            <input
                value={form.name}
                onChange={(e) =>
                    setForm({
                        ...form,
                        name: e.target.value
                    })
                }
            />

            <input
                value={form.email}
                onChange={(e) =>
                    setForm({
                        ...form,
                        email: e.target.value
                    })
                }
            />
        </div>
    );
}
```

### Important

```jsx
...form
```

keeps the existing values.

Then we update only the required property.

---

# 8. Controlled Components

A form input controlled by React state is called a **controlled component**.

Example:

```jsx
const [name, setName] = useState("");

<input
    value={name}
    onChange={(e) => setName(e.target.value)}
/>
```

Here:

```text
React State
    ↕
Input
```

React controls the input value.

### Simple Definition

> A controlled component is a form element whose value is controlled by React state.

---

# 9. Uncontrolled Components

An uncontrolled component stores its value in the DOM instead of React state.

Usually, we access it using `useRef`.

Example:

```jsx
import { useRef } from "react";

function App() {

    const inputRef = useRef();

    function handleSubmit(event) {

        event.preventDefault();

        console.log(inputRef.current.value);
    }

    return (
        <form onSubmit={handleSubmit}>

            <input ref={inputRef} />

            <button type="submit">
                Submit
            </button>

        </form>
    );
}
```

We will learn `useRef` in detail later.

---

# 10. Checkbox

Checkboxes are commonly handled using `checked`.

```jsx
import { useState } from "react";

function App() {

    const [accepted, setAccepted] = useState(false);

    return (
        <label>

            <input
                type="checkbox"
                checked={accepted}
                onChange={(e) => setAccepted(e.target.checked)}
            />

            I accept the terms

        </label>
    );
}
```

Important:

```jsx
e.target.checked
```

returns:

```text
true
```

or:

```text
false
```

---

# 11. Select Dropdown

```jsx
import { useState } from "react";

function App() {

    const [course, setCourse] = useState("");

    return (
        <select
            value={course}
            onChange={(e) => setCourse(e.target.value)}
        >

            <option value="">
                Select Course
            </option>

            <option value="java">
                Java
            </option>

            <option value="react">
                React
            </option>

            <option value="python">
                Python
            </option>

        </select>
    );
}
```

---

# 12. Radio Buttons

```jsx
import { useState } from "react";

function App() {

    const [gender, setGender] = useState("");

    return (
        <div>

            <label>
                <input
                    type="radio"
                    value="male"
                    checked={gender === "male"}
                    onChange={(e) => setGender(e.target.value)}
                />
                Male
            </label>

            <label>
                <input
                    type="radio"
                    value="female"
                    checked={gender === "female"}
                    onChange={(e) => setGender(e.target.value)}
                />
                Female
            </label>

        </div>
    );
}
```

---

# 13. Form Validation

We can check whether the user entered valid data before submitting.

```jsx
function handleSubmit(event) {

    event.preventDefault();

    if (name === "") {
        alert("Name is required");
        return;
    }

    if (email === "") {
        alert("Email is required");
        return;
    }

    console.log("Form submitted");
}
```

### Flow

```text
Submit
  ↓
Validate
  ↓
Valid? ── No → Show Error
  |
 Yes
  ↓
Submit Data
```

---

# 14. Example – Login Form

```jsx
import { useState } from "react";

function App() {

    const [email, setEmail] = useState("");
    const [password, setPassword] = useState("");

    function handleSubmit(event) {

        event.preventDefault();

        if (!email || !password) {
            alert("Please fill all fields");
            return;
        }

        console.log("Login successful");
    }

    return (
        <form onSubmit={handleSubmit}>

            <input
                type="email"
                placeholder="Email"
                value={email}
                onChange={(e) => setEmail(e.target.value)}
            />

            <input
                type="password"
                placeholder="Password"
                value={password}
                onChange={(e) => setPassword(e.target.value)}
            />

            <button type="submit">
                Login
            </button>

        </form>
    );
}
```

---

# 15. Common Form Events

| Event | Purpose |
|---|---|
| `onChange` | Detect input changes |
| `onSubmit` | Handle form submission |
| `onFocus` | Input receives focus |
| `onBlur` | Input loses focus |
| `onKeyDown` | Keyboard key pressed |

---

# 16. Important Input Properties

### Text Input

```jsx
value={name}
```

### Checkbox

```jsx
checked={accepted}
```

### Select

```jsx
value={course}
```

### Input Change

```jsx
onChange={(e) => setName(e.target.value)}
```

---

# 🧠 Easy Memory Trick

```text
Input
  ↓
onChange
  ↓
State
  ↓
User submits
  ↓
onSubmit
  ↓
Validation
  ↓
Process data
```

Remember:

```text
value     → Input value
checked   → Checkbox value
onChange  → Input changes
onSubmit  → Form submission
```

---

# 🎯 Interview One-Liners

### What is form handling in React?

> Form handling is the process of managing user input, validation, and submission using React.

### What is a controlled component?

> A controlled component is a form element whose value is controlled by React state.

### Why do we use `event.preventDefault()`?

> It prevents the browser's default form submission behavior.

### How do you get an input value?

```jsx
event.target.value
```

### How do you get a checkbox value?

```jsx
event.target.checked
```

### Controlled vs Uncontrolled?

| Controlled | Uncontrolled |
|---|---|
| React state controls value | DOM controls value |
| Uses `value` / `checked` | Often uses `ref` |
| Easy validation and dynamic UI | Useful for simpler cases |
| Common in React forms | Less common |

---

# 🔥 Quick Revision

```text
Forms
→ Collect user input

onChange
→ Track input changes

onSubmit
→ Handle form submission

preventDefault()
→ Prevent browser default behavior

Controlled Component
→ React state controls input

Uncontrolled Component
→ DOM manages input value

Validation
→ Check input before processing
```

# ⭐ Key Point

> **In React, forms are commonly handled using controlled components, where input values are stored and updated through React state.**
