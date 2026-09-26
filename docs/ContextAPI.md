# 📘 React – Context API

## 1. What is Context API?

**Context API** is a React feature used to share data between components without manually passing props through every level.

It is useful when many components need access to the same data.

### Simple Definition

> Context API allows data to be shared across the component tree without prop drilling.

---

# 2. What is Prop Drilling?

**Prop drilling** means passing props through intermediate components even when those components don't need the data.

Example:

```text
App
 ↓ user
Parent
 ↓ user
Child
 ↓ user
GrandChild
```

Suppose only `GrandChild` needs `user`.

Still, `App → Parent → Child → GrandChild` must pass it.

This is called **prop drilling**.

---

# 3. Context API Solves Prop Drilling

Instead of:

```text
App
 ↓ props
Parent
 ↓ props
Child
 ↓ props
GrandChild
```

We can use Context:

```text
          Context
         ↙   ↓   ↘
      Parent Child GrandChild
```

The required component can directly access the context.

---

# 4. Main Parts of Context API

There are three important steps:

```text
1. createContext()
        ↓
2. Provider
        ↓
3. useContext()
```

Flow:

```text
createContext()
      ↓
Provider
      ↓
Shared Data
      ↓
useContext()
      ↓
Component
```

---

# 5. Step 1 – createContext()

Import:

```jsx
import { createContext } from "react";
```

Create a context:

```jsx
const UserContext = createContext();
```

We can also provide a default value:

```jsx
const UserContext = createContext("Guest");
```

---

# 6. Step 2 – Provider

The Provider makes data available to components inside it.

Example:

```jsx
<UserContext.Provider value="Dharun">
  <Child />
</UserContext.Provider>
```

Here:

```jsx
value="Dharun"
```

is the data being shared.

---

# 7. Step 3 – useContext()

The child component can access the context using:

```jsx
import { useContext } from "react";

const user = useContext(UserContext);
```

Now:

```jsx
console.log(user);
```

Output:

```text
Dharun
```

---

# 8. Complete Simple Example

```jsx
import {
  createContext,
  useContext
} from "react";

const UserContext = createContext();

function App() {
  return (
    <UserContext.Provider value="Dharun">
      <Profile />
    </UserContext.Provider>
  );
}

function Profile() {
  const user = useContext(UserContext);

  return <h1>Hello {user}</h1>;
}

export default App;
```

Output:

```text
Hello Dharun
```

---

# 9. How It Works

```text
App
 ↓
UserContext.Provider
 ↓
value = "Dharun"
 ↓
Profile
 ↓
useContext(UserContext)
 ↓
"Dharun"
```

No props are required between `App` and `Profile`.

---

# 10. Context with Multiple Components

```jsx
const ThemeContext = createContext();

function App() {
  return (
    <ThemeContext.Provider value="dark">
      <Header />
      <Main />
      <Footer />
    </ThemeContext.Provider>
  );
}
```

Now all these components can access:

```text
Header
Main
Footer
```

using:

```jsx
useContext(ThemeContext);
```

---

# 11. Context with an Object

The Provider can share an object.

```jsx
const UserContext = createContext();

function App() {
  const user = {
    name: "Dharun",
    role: "Developer"
  };

  return (
    <UserContext.Provider value={user}>
      <Profile />
    </UserContext.Provider>
  );
}
```

Consume it:

```jsx
function Profile() {
  const user = useContext(UserContext);

  return (
    <>
      <h1>{user.name}</h1>
      <p>{user.role}</p>
    </>
  );
}
```

Output:

```text
Dharun
Developer
```

---

# 12. Context with State

Context can share state and its setter.

```jsx
import {
  createContext,
  useContext,
  useState
} from "react";

const CounterContext = createContext();

function App() {
  const [count, setCount] = useState(0);

  return (
    <CounterContext.Provider
      value={{ count, setCount }}
    >
      <Counter />
    </CounterContext.Provider>
  );
}

function Counter() {
  const { count, setCount } = useContext(CounterContext);

  return (
    <>
      <h1>{count}</h1>

      <button onClick={() => setCount(prev => prev + 1)}>
        Increase
      </button>
    </>
  );
}
```

Here Context shares:

```text
count
setCount
```

---

# 13. Common Uses of Context API

Context is commonly used for application-wide or widely shared data such as:

```text
Authentication
Theme
Language
User information
Application settings
Permissions
```

Example:

```text
AuthContext
ThemeContext
LanguageContext
```

---

# 14. Context vs Props

| Feature | Props | Context |
|---|---|---|
| Data passed by | Parent | Provider |
| Prop drilling | Possible | Helps avoid |
| Best for | Direct parent-child data | Widely shared data |
| Access | Component parameter | `useContext()` |
| Setup | Simple | Requires Context setup |

### Memory

```text
Props
→ Parent → Child

Context
→ Shared data → Many components
```

---

# 15. Context vs useState

These are not alternatives in the same sense.

### useState

Used to:

```text
Create and manage state
```

Example:

```jsx
const [name, setName] = useState("Dharun");
```

### Context

Used to:

```text
Make data available to many components
```

Example:

```jsx
<UserContext.Provider value={name}>
```

They are often used together.

```text
useState
   ↓
Stores state
   ↓
Context
   ↓
Shares state
```

---

# 16. Context + useState

A common pattern:

```jsx
const [theme, setTheme] = useState("light");

<ThemeContext.Provider
  value={{ theme, setTheme }}
>
  <App />
</ThemeContext.Provider>
```

Now deeply nested components can:

```jsx
const { theme, setTheme } = useContext(ThemeContext);
```

---

# 17. Creating a Separate Context File

In real projects, Context is often separated into its own file.

Example:

```text
src/
├── context/
│   └── UserContext.jsx
├── components/
│   ├── Header.jsx
│   └── Profile.jsx
└── App.jsx
```

### UserContext.jsx

```jsx
import { createContext } from "react";

export const UserContext = createContext();
```

### App.jsx

```jsx
import { UserContext } from "./context/UserContext";

function App() {
  return (
    <UserContext.Provider value="Dharun">
      <Profile />
    </UserContext.Provider>
  );
}
```

---

# 18. Important Provider Rule

A component must be **inside the Provider** to receive the Provider's value.

Example:

```jsx
<UserContext.Provider value="Dharun">
  <Profile />
</UserContext.Provider>
```

`Profile` can access:

```jsx
useContext(UserContext);
```

But a component outside the Provider will not receive that Provider value.

---

# 19. Context Default Value

Example:

```jsx
const UserContext = createContext("Guest");
```

If there is no matching Provider above the component:

```jsx
const user = useContext(UserContext);
```

the default value can be returned:

```text
Guest
```

---

# 20. Context and Re-rendering

When the Provider's context value changes, components consuming that context can re-render.

Example:

```jsx
<ThemeContext.Provider value={theme}>
```

If:

```text
theme = "light"
```

changes to:

```text
theme = "dark"
```

components consuming that context can update.

---

# 21. Context API vs Prop Drilling

### Prop Drilling

```text
App
 ↓ user
Parent
 ↓ user
Child
 ↓ user
Profile
```

### Context

```text
          UserContext
          ↙    ↓    ↘
       Parent Child Profile
```

The `Profile` component can directly access the shared value.

---

# 22. Common Mistakes

### ❌ Mistake 1: Forgetting Provider

```jsx
const user = useContext(UserContext);
```

without providing the required context value.

---

### ❌ Mistake 2: Using Context for everything

Not every piece of data needs Context.

For simple parent-child communication:

```text
Props
```

may be better.

---

### ❌ Mistake 3: Confusing Context with state management

Context mainly provides a way to **share values**.

It does not automatically replace state management.

Often:

```text
useState + Context
```

are used together.

---

### ❌ Mistake 4: Putting too much frequently changing data into one Context

When context values change frequently, many consumers may re-render.

For larger applications, state management and context structure should be designed carefully.

---

# 23. Context API Flow

```text
createContext()
       ↓
Create Context
       ↓
Provider
       ↓
Provide value
       ↓
Child component
       ↓
useContext()
       ↓
Read value
```

---

# 24. Interview Questions

### Q1. What is Context API?

> Context API is a React feature that allows data to be shared across components without passing props through every level.

### Q2. What problem does Context solve?

It mainly helps solve **prop drilling**.

### Q3. What are the main parts of Context API?

```text
createContext()
Provider
useContext()
```

### Q4. Can Context share state?

Yes.

For example:

```jsx
value={{ count, setCount }}
```

can share both state and its updater.

### Q5. Is Context a replacement for useState?

No.

`useState` manages state, while Context provides a way to share values across the component tree.

### Q6. What are common uses of Context?

```text
Authentication
Theme
Language
User information
Global settings
```

---

# 25. Quick Revision

```text
Context API
     ↓
Avoid Prop Drilling
     ↓
createContext()
     ↓
Provider
     ↓
Shared Value
     ↓
useContext()
     ↓
Component
```

### Easy Memory

```text
create → Provide → Consume
```

```text
createContext()
      ↓
Provider
      ↓
useContext()
```

# 🎯 Interview One-Liner

> **Context API allows React components to share data across the component tree without manually passing props through every level.**

# ⭐ Key Point

```text
Props   → Good for direct component communication
Context → Good for widely shared data
```
