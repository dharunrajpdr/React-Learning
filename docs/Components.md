# 📘 React – Components

## 1. What is a Component?

A **component** is a reusable piece of UI in a React application.

### Simple Definition

> A component is a JavaScript function that returns UI.

Example:

```jsx
function Welcome() {
    return <h1>Hello React</h1>;
}
```

---

# 2. Why Do We Use Components?

Components help us:

- Reuse UI
- Organize code
- Reduce duplicate code
- Make applications easier to maintain
- Divide a large UI into smaller parts

---

# 3. Simple Component

```jsx
function Header() {
    return <h1>My Website</h1>;
}
```

Use it inside another component:

```jsx
function App() {
    return (
        <div>
            <Header />
        </div>
    );
}
```

### Output

```text
My Website
```

---

# 4. Component Naming Rule

React component names should normally start with an **uppercase letter**.

### Correct

```jsx
function Header() {
    return <h1>Header</h1>;
}
```

### Incorrect

```jsx
function header() {
    return <h1>Header</h1>;
}
```

Use:

```jsx
<Header />
```

not:

```jsx
<header />
```

Lowercase names are generally treated as HTML elements.

---

# 5. Functional Components

Modern React mainly uses **function components**.

Example:

```jsx
function Welcome() {
    return <h1>Welcome to React</h1>;
}

export default Welcome;
```

Another example:

```jsx
const Welcome = () => {
    return <h1>Welcome to React</h1>;
};
```

Both are function components.

---

# 6. Reusing Components

A component can be reused multiple times.

```jsx
function Button() {
    return <button>Click Me</button>;
}

function App() {
    return (
        <div>
            <Button />
            <Button />
            <Button />
        </div>
    );
}
```

### Output

```text
[Click Me]

[Click Me]

[Click Me]
```

The same component is used three times.

---

# 7. Component Tree

React applications can contain components inside other components.

Example:

```text
App
│
├── Header
│
├── Navbar
│
├── Main
│   ├── Card
│   └── Card
│
└── Footer
```

This is called a **component tree**.

---

# 8. Parent and Child Components

If one component uses another component, we can call it a **parent-child relationship**.

Example:

```jsx
function Child() {
    return <p>I am Child</p>;
}

function Parent() {
    return (
        <div>
            <h1>I am Parent</h1>
            <Child />
        </div>
    );
}
```

Here:

```text
Parent
  ↓
Child
```

`Parent` is the parent component.

`Child` is the child component.

---

# 9. Passing Data Between Components

A parent component can pass data to a child using **props**.

Example:

```jsx
function Student(props) {
    return <h2>{props.name}</h2>;
}

function App() {
    return (
        <div>
            <Student name="Dharun" />
            <Student name="Ajay" />
        </div>
    );
}
```

### Output

```text
Dharun
Ajay
```

We will learn **props** in the next topic.

---

# 10. Component with JavaScript

A component can contain JavaScript variables and logic.

```jsx
function Student() {

    const name = "Dharun";
    const age = 21;

    return (
        <div>
            <h2>{name}</h2>
            <p>Age: {age}</p>
        </div>
    );
}
```

---

# 11. Component with CSS

Example:

```jsx
function Button() {
    return (
        <button className="btn">
            Click Me
        </button>
    );
}
```

CSS:

```css
.btn {
    padding: 10px 20px;
}
```

---

# 12. Exporting a Component

We can export a component:

```jsx
function Header() {
    return <h1>My Website</h1>;
}

export default Header;
```

Then import it:

```jsx
import Header from "./Header";

function App() {
    return <Header />;
}
```

---

# 13. Default Export vs Named Export

### Default Export

```jsx
export default Header;
```

Import:

```jsx
import Header from "./Header";
```

---

### Named Export

```jsx
export function Header() {
    return <h1>Header</h1>;
}
```

Import:

```jsx
import { Header } from "./Header";
```

---

# 14. Real-World Example

Imagine an e-commerce website.

Instead of creating everything in one file:

```text
App.jsx
```

We can divide it:

```text
components/
│
├── Navbar.jsx
├── ProductCard.jsx
├── Button.jsx
├── Footer.jsx
└── SearchBar.jsx
```

Then:

```text
App
│
├── Navbar
├── SearchBar
├── ProductCard
├── ProductCard
└── Footer
```

This makes the application easier to manage.

---

# 15. Component vs Normal Function

A React component is also a JavaScript function, but it is designed to return UI.

### Normal Function

```javascript
function add(a, b) {
    return a + b;
}
```

### React Component

```jsx
function Welcome() {
    return <h1>Hello React</h1>;
}
```

---

# 🧠 Easy Memory Trick

```text
Component
   ↓
Reusable UI
   ↓
JavaScript Function
   ↓
Returns JSX
```

---

# 🎯 Interview One-Liners

### What is a component?

> A component is a reusable piece of UI that is usually created using a JavaScript function and returns JSX.

### Why are components used?

> Components help us create reusable, organized, and maintainable UI.

### What is a functional component?

> A functional component is a JavaScript function that returns React UI.

### How should React component names start?

> React component names should normally start with an uppercase letter.

### What is a component tree?

> A component tree represents the parent-child structure of components in a React application.

### How does a parent pass data to a child?

> A parent passes data to a child using props.

---

# 🔥 Quick Revision

```text
Component
→ Reusable UI
→ Usually a JavaScript function
→ Returns JSX
→ Name starts with uppercase
→ Can be reused multiple times
→ Can contain child components
→ Parent can pass data using props
```

### Example

```jsx
function App() {
    return (
        <div>
            <Header />
            <Main />
            <Footer />
        </div>
    );
}
```

```text
App
├── Header
├── Main
└── Footer
```

# ⭐ Key Point

> **React applications are built by combining small, reusable components to create a complete user interface.**
