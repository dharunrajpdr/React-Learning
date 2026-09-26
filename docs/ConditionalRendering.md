# 📘 React – Conditional Rendering

## 1. What is Conditional Rendering?

**Conditional rendering** means displaying different UI elements based on a condition.

### Simple Definition

> Conditional rendering allows React to show different components or elements depending on a condition.

Example:

```text
User logged in
      ↓
Show Dashboard

User not logged in
      ↓
Show Login
```

---

# 2. Using `if-else`

We can use normal JavaScript `if-else` inside a component.

```jsx
function App() {

    const isLoggedIn = true;

    if (isLoggedIn) {
        return <h1>Welcome User</h1>;
    } else {
        return <h1>Please Login</h1>;
    }
}
```

### Output

If:

```js
isLoggedIn = true
```

Output:

```text
Welcome User
```

---

# 3. Using Ternary Operator

The ternary operator is commonly used directly inside JSX.

### Syntax

```jsx
condition ? trueValue : falseValue
```

Example:

```jsx
function App() {

    const isLoggedIn = true;

    return (
        <div>
            {isLoggedIn ? (
                <h1>Welcome User</h1>
            ) : (
                <h1>Please Login</h1>
            )}
        </div>
    );
}
```

### Flow

```text
isLoggedIn
    ↓
true  → Welcome User
false → Please Login
```

---

# 4. Using Logical AND `&&`

We can use `&&` when we want to display something only when a condition is true.

```jsx
function App() {

    const isAdmin = true;

    return (
        <div>

            <h1>Dashboard</h1>

            {isAdmin && <button>Admin Panel</button>}

        </div>
    );
}
```

If:

```js
isAdmin = true
```

Output:

```text
Dashboard
Admin Panel
```

If:

```js
isAdmin = false
```

Output:

```text
Dashboard
```

---

# 5. `if-else` vs Ternary vs `&&`

| Method | Use |
|---|---|
| `if-else` | Larger/complex conditions |
| Ternary `? :` | Two possible UI outputs |
| `&&` | Show something only when condition is true |

---

# 6. Conditional Rendering with State

Conditional rendering is often used with state.

```jsx
import { useState } from "react";

function App() {

    const [isLoggedIn, setIsLoggedIn] = useState(false);

    return (
        <div>

            {isLoggedIn ? (
                <h1>Welcome User</h1>
            ) : (
                <h1>Please Login</h1>
            )}

            <button onClick={() => setIsLoggedIn(!isLoggedIn)}>
                Login / Logout
            </button>

        </div>
    );
}
```

### Flow

```text
Button Click
     ↓
State Changes
     ↓
isLoggedIn changes
     ↓
React re-renders
     ↓
Different UI appears
```

---

# 7. Multiple Conditions

Suppose we have three user roles:

```text
admin
user
guest
```

We can use multiple conditions.

```jsx
function App() {

    const role = "admin";

    return (
        <div>

            {role === "admin" && <h1>Admin Dashboard</h1>}

            {role === "user" && <h1>User Dashboard</h1>}

            {role === "guest" && <h1>Guest Page</h1>}

        </div>
    );
}
```

---

# 8. Nested Ternary

It is possible to use multiple ternary operators.

```jsx
function App() {

    const role = "admin";

    return (
        <h1>
            {role === "admin"
                ? "Admin"
                : role === "user"
                ? "User"
                : "Guest"}
        </h1>
    );
}
```

### Note

Nested ternaries can become difficult to read.

For complex conditions, prefer:

```text
if-else
```

or separate components/functions.

---

# 9. Conditional Rendering with Components

We can conditionally render components.

```jsx
function Login() {
    return <h2>Login Page</h2>;
}

function Dashboard() {
    return <h2>Dashboard</h2>;
}

function App() {

    const isLoggedIn = true;

    return (
        <div>
            {isLoggedIn ? <Dashboard /> : <Login />}
        </div>
    );
}
```

---

# 10. Conditional CSS Class

We can also conditionally apply classes.

```jsx
function App() {

    const isActive = true;

    return (
        <button className={isActive ? "active" : "inactive"}>
            Status
        </button>
    );
}
```

If `isActive` is true:

```text
class = active
```

Otherwise:

```text
class = inactive
```

---

# 11. Example – Show Loading

A common real-world example is showing a loading message.

```jsx
function App() {

    const loading = true;

    return (
        <div>
            {loading ? (
                <h2>Loading...</h2>
            ) : (
                <h2>Data Loaded</h2>
            )}
        </div>
    );
}
```

---

# 12. Example – Login Button

```jsx
function App() {

    const isLoggedIn = false;

    return (
        <div>

            {isLoggedIn ? (
                <button>Logout</button>
            ) : (
                <button>Login</button>
            )}

        </div>
    );
}
```

---

# 13. Important: Don't Use `if` Directly Inside JSX

❌ This is invalid:

```jsx
return (
    <div>

        if (isLoggedIn) {
            <h1>Welcome</h1>
        }

    </div>
);
```

Instead, use:

```jsx
{isLoggedIn && <h1>Welcome</h1>}
```

or:

```jsx
{isLoggedIn ? <h1>Welcome</h1> : <h1>Login</h1>}
```

---

# 14. Common Real-World Uses

Conditional rendering is commonly used for:

```text
Login / Logout
Loading / Loaded
Error / Success
Admin / User
Dark / Light mode
Empty / Non-empty list
Show / Hide modal
Permission-based UI
```

---

# 🧠 Easy Memory Trick

```text
Condition
   ↓
true  → Show A
false → Show B
```

### Three main ways:

```text
if-else
   ↓
Complex logic

Ternary
   ↓
A or B

&&
   ↓
Show only when true
```

---

# 🎯 Interview One-Liners

### What is conditional rendering?

> Conditional rendering means displaying UI elements based on a condition.

### How can you perform conditional rendering in React?

> Using `if-else`, ternary operators, logical `&&`, and conditional components.

### What is the ternary operator?

> It is a shorthand for choosing between two values based on a condition.

```jsx
condition ? trueValue : falseValue
```

### When do we use `&&`?

> We use `&&` when we want to render something only if a condition is true.

### Can we use `if` directly inside JSX?

> No. We generally use `if-else` before the JSX return, or use ternary and logical operators inside JSX.

---

# 🔥 Quick Revision

```text
Conditional Rendering
→ Show UI based on condition

if-else
→ Complex conditions

Ternary
→ condition ? A : B

&&
→ condition && component

Example:

{isLoggedIn
    ? <Dashboard />
    : <Login />
}
```

# ⭐ Key Point

> **Conditional rendering allows React to dynamically display different UI based on application state or other conditions.**
