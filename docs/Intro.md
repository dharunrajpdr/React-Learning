# 📘 React – Introduction

## 1. What is React?

**React** is a **JavaScript library** used to build **user interfaces (UI)**, especially for web applications.

React was developed by **Facebook (now Meta)**.

### Simple Definition

> React is a JavaScript library used to build fast, interactive, and reusable user interfaces.

---

## 2. Why Do We Use React?

In a normal website, changing one small part of the page may require updating the DOM manually.

React makes UI development easier by:

- Creating reusable components
- Managing changing data
- Updating the UI efficiently
- Making large applications easier to maintain

---

## 3. React is a Library

React is officially a **JavaScript library**, not a complete framework.

React mainly focuses on the **UI layer**.

Other features can be added using libraries such as:

```text
React Router → Routing
Axios        → API requests
Redux        → State management
```

---

## 4. Main Features of React

### 1. Components

React applications are built using reusable **components**.

```jsx
function Welcome() {
    return <h1>Hello React</h1>;
}
```

---

### 2. JSX

JSX allows us to write HTML-like syntax inside JavaScript.

```jsx
const element = <h1>Hello World</h1>;
```

---

### 3. Reusable UI

A component can be created once and reused multiple times.

```jsx
function Button() {
    return <button>Click Me</button>;
}
```

---

### 4. State Management

React can store and manage changing data using **state**.

Example:

```jsx
const [count, setCount] = useState(0);
```

When the state changes, React updates the required UI.

---

### 5. Virtual DOM

React uses a **Virtual DOM** to efficiently update the actual DOM.

Simple idea:

```text
State Changes
     ↓
React creates updated Virtual DOM
     ↓
Compares with previous Virtual DOM
     ↓
Updates required DOM changes
```

---

## 5. React Example

```jsx
function App() {
    return (
        <div>
            <h1>Hello React</h1>
            <p>Welcome to React.</p>
        </div>
    );
}

export default App;
```

### Output

```text
Hello React

Welcome to React.
```

---

## 6. React Component

A **component** is a reusable piece of UI.

Example:

```jsx
function Student() {
    return <h2>Dharun</h2>;
}
```

We can use it inside another component:

```jsx
function App() {
    return (
        <div>
            <Student />
            <Student />
        </div>
    );
}
```

The `Student` component is reused two times.

---

## 7. React Application Structure

A simple React application can look like:

```text
my-react-app/
│
├── src/
│   ├── App.jsx
│   ├── main.jsx
│   └── components/
│
├── public/
├── package.json
└── node_modules/
```

We will learn each part in detail later.

---

## 8. React vs JavaScript

| JavaScript | React |
|---|---|
| Programming language | JavaScript library |
| Can manipulate DOM directly | Provides component-based UI development |
| General purpose | Mainly used for UI |
| Can build web applications | Makes UI development easier |
| No component system by default | Component-based |

### Important

React is **not a replacement for JavaScript**.

React is built using JavaScript.

```text
JavaScript
    ↓
React
    ↓
Build Interactive UI
```

---

## 9. Advantages of React

- Reusable components
- Easy UI development
- Efficient UI updates
- Large ecosystem
- Easy integration with APIs
- Good for single-page applications
- Large community
- Widely used in frontend development

---

## 10. Where is React Used?

React can be used to build:

```text
Websites
↓
Dashboards
↓
E-commerce applications
↓
Social media applications
↓
Chat applications
↓
Admin panels
↓
Single Page Applications (SPA)
```

---

## 11. React in MERN Stack

React is the **frontend** technology in the MERN stack.

```text
M → MongoDB
E → Express.js
R → React.js
N → Node.js
```

### Architecture

```text
User
 ↓
React Frontend
 ↓
API Request
 ↓
Node.js + Express.js
 ↓
MongoDB
```

---

# 🧠 Easy Memory Trick

```text
React
 ↓
JavaScript Library
 ↓
Build UI
 ↓
Components
 ↓
Reusable + Interactive
```

---

# 🎯 Interview One-Liners

### What is React?

> React is a JavaScript library used to build interactive and reusable user interfaces.

### Who developed React?

> React was developed by Facebook, now known as Meta.

### Is React a framework?

> React is officially a JavaScript library focused mainly on building user interfaces.

### What is a component?

> A component is a reusable piece of UI in a React application.

### What is JSX?

> JSX is a syntax extension for JavaScript that allows us to write HTML-like code inside JavaScript.

### What is Virtual DOM?

> Virtual DOM is a lightweight representation of the actual DOM that React uses to efficiently update the UI.

---

# 🔥 Quick Revision

```text
React
→ JavaScript library
→ Used for building UI
→ Developed by Facebook/Meta
→ Component-based
→ Uses JSX
→ Supports reusable components
→ Uses state for dynamic data
→ Uses Virtual DOM for efficient UI updates
→ Used as frontend in MERN
```

# ⭐ Key Point

> **React = JavaScript Library + Components + JSX + State + Efficient UI Updates**
