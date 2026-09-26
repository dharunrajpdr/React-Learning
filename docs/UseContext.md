# 📘 React – useContext Hook

## 1. What is `useContext`?

`useContext` is a React Hook used to share data between multiple components **without manually passing props through every level**.

### Simple Definition

> `useContext` allows components to access shared data directly from a Context.

---

# 2. Why Do We Need `useContext`?

Suppose we have:

```text
App
 ↓
Parent
 ↓
Child
 ↓
GrandChild
```

If `App` has some data that `GrandChild` needs, we might have to pass it like:

```text
App
 ↓ props
Parent
 ↓ props
Child
 ↓ props
GrandChild
```

This is called **Prop Drilling**.

`useContext` helps avoid unnecessary prop drilling.

---

# 3. What is Prop Drilling?

Prop drilling means passing props through components that don't actually need the data.

Example:

```jsx
<App user={user} />
```

Then:

```jsx
<Parent user={user} />
```

Then:

```jsx
<Child user={user} />
```

Then:

```jsx
<GrandChild user={user} />
```

Only `GrandChild` needs `user`.

This can become difficult in large applications.

---

# 4. Context API

React provides the Context API to share data.

The basic flow is:

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

# 5. Creating a Context

```jsx
import { createContext } from "react";

const UserContext = createContext();
```

Here:

```text
UserContext
→ Context object
```

---

# 6. Context Provider

The Provider makes data available to components inside it.

```jsx
<UserContext.Provider value="Dharun">

    <Child />

</UserContext.Provider>
```

The value:

```jsx
"Dharun"
```

can be accessed by components inside the Provider.

---

# 7. Using `useContext`

```jsx
import { useContext } from "react";

function Child() {

    const user = useContext(UserContext);

    return <h2>Hello {user}</h2>;
}
```

Now `Child` can directly access:

```text
Dharun
```

without receiving it through props.

---

# 8. Complete Example

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

    return (
        <h1>
            Welcome {user}
        </h1>
    );
}

export default App;
```

### Output

```text
Welcome Dharun
```

---

# 9. Context with Multiple Components

```jsx
import {
    createContext,
    useContext
} from "react";

const UserContext = createContext();

function App() {

    return (
        <UserContext.Provider value="Dharun">

            <Navbar />
            <Profile />

        </UserContext.Provider>
    );
}

function Navbar() {

    const user = useContext(UserContext);

    return <h2>Navbar: {user}</h2>;
}

function Profile() {

    const user = useContext(UserContext);

    return <h2>Profile: {user}</h2>;
}
```

Both components can access the same context.

---

# 10. Context with an Object

We can provide multiple values.

```jsx
const user = {
    name: "Dharun",
    role: "Student"
};
```

Then:

```jsx
<UserContext.Provider value={user}>
    <Profile />
</UserContext.Provider>
```

Access it:

```jsx
function Profile() {

    const user = useContext(UserContext);

    return (
        <div>

            <h2>{user.name}</h2>
            <p>{user.role}</p>

        </div>
    );
}
```

---

# 11. Context with State

Context becomes more useful when combined with state.

```jsx
import {
    createContext,
    useContext,
    useState
} from "react";

const UserContext = createContext();

function App() {

    const [user, setUser] = useState("Dharun");

    return (
        <UserContext.Provider value={{ user, setUser }}>

            <Profile />

        </UserContext.Provider>
    );
}

function Profile() {

    const { user, setUser } = useContext(UserContext);

    return (
        <div>

            <h2>{user}</h2>

            <button onClick={() => setUser("Ajay")}>
                Change User
            </button>

        </div>
    );
}
```

Now the child can both:

```text
Read data
+
Update data
```

---

# 12. Real-World Uses

Context is commonly used for data that many components need.

Examples:

```text
Theme
    ↓
Light / Dark mode

Authentication
    ↓
Current user

Language
    ↓
English / Tamil / Hindi

Global settings
    ↓
Application preferences
```

---

# 13. Context vs Props

| Props | Context |
|---|---|
| Pass data parent → child | Share data across component tree |
| Good for direct relationships | Useful for shared data |
| Can cause prop drilling | Helps avoid prop drilling |
| Explicit data flow | Components can consume context directly |

---

# 14. Context vs `useState`

These are not exactly replacements for each other.

### `useState`

Used to:

```text
Create and manage state
```

### Context

Used to:

```text
Make data available to many components
```

They are often used together:

```text
useState
   ↓
Manage data

Context
   ↓
Share data
```

---

# 15. Provider Must Wrap the Consumer

This works:

```jsx
<UserContext.Provider value="Dharun">

    <Profile />

</UserContext.Provider>
```

Because `Profile` is inside the Provider.

The context value is available to descendants of the Provider.

---

# 16. Default Context Value

We can provide a default value:

```jsx
const UserContext = createContext("Guest");
```

If a component consumes the context without a matching Provider above it, it receives:

```text
Guest
```

---

# 17. Common Mistake

### ❌ Creating Context but not providing it

```jsx
const UserContext = createContext();

function Profile() {

    const user = useContext(UserContext);

}
```

If there is no Provider above `Profile`, the component gets the context's default value.

### ✅ Correct

```jsx
<UserContext.Provider value="Dharun">
    <Profile />
</UserContext.Provider>
```

---

# 18. Context Flow

```text
createContext()
      ↓
Create Context

Provider
      ↓
Provide value

useContext()
      ↓
Consume value

Component
      ↓
Use shared data
```

---

# 🧠 Easy Memory Trick

Remember:

```text
Props
→ Pass data

Context
→ Share data

useContext
→ Get shared data
```

And:

```text
Context
   ↓
Provider
   ↓
useContext()
   ↓
Component
```

---

# 🎯 Interview One-Liners

### What is `useContext`?

> `useContext` is a React Hook used to consume data from a Context without manually passing props through every component level.

### What is Prop Drilling?

> Prop drilling is passing data through multiple intermediate components just to reach a deeply nested component.

### What is Context API?

> Context API provides a way to share data across a component tree without passing props manually through every level.

### What is a Provider?

> A Provider supplies a context value to its descendant components.

### When should we use Context?

> Context is useful for shared data such as authentication, themes, language, and global application settings.

### Is Context a replacement for `useState`?

> No. `useState` manages state, while Context provides a way to share data across components.

---

# 🔥 Quick Revision

```text
useContext
→ Access shared context data

createContext()
→ Create context

Provider
→ Provide data

useContext()
→ Consume data

Main benefit
→ Avoid prop drilling

Common uses
→ Auth
→ Theme
→ Language
→ Global settings
```

# ⭐ Key Point

> **`useContext` makes shared data accessible to components without manually passing props through every level of the component tree.**
