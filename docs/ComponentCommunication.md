# 📘 React – Component Communication

## 1. What is Component Communication?

**Component communication** means sharing data or triggering actions between React components.

Example:

```text
Parent Component
      ↓
Child Component
```

The most common way is:

```text
Parent → Child
```

using **props**.

---

# 2. Why Do Components Need to Communicate?

In a real React application, different components need to share information.

Example:

```text
App
 ├── Navbar
 ├── ProductList
 │    └── ProductCard
 └── Footer
```

A product might need to communicate:

```text
ProductList → ProductCard
ProductCard → ProductList
```

---

# 3. Parent → Child

The most common communication pattern is:

```text
Parent
  ↓
Props
  ↓
Child
```

Example:

```jsx
function App() {

    return (
        <Student name="Dharun" />
    );
}
```

Child:

```jsx
function Student(props) {

    return (
        <h2>Hello {props.name}</h2>
    );
}
```

Output:

```text
Hello Dharun
```

---

# 4. Passing Multiple Props

```jsx
function App() {

    return (
        <Student
            name="Dharun"
            age={21}
            course="CSE"
        />
    );
}
```

Child:

```jsx
function Student(props) {

    return (
        <div>

            <h2>{props.name}</h2>

            <p>{props.age}</p>

            <p>{props.course}</p>

        </div>
    );
}
```

---

# 5. Passing Props Using Destructuring

Instead of:

```jsx
function Student(props) {

    return <h2>{props.name}</h2>;
}
```

We can write:

```jsx
function Student({ name }) {

    return <h2>{name}</h2>;
}
```

For multiple props:

```jsx
function Student({
    name,
    age,
    course
}) {

    return (
        <div>

            <h2>{name}</h2>
            <p>{age}</p>
            <p>{course}</p>

        </div>
    );
}
```

---

# 6. Passing Functions as Props

A parent can pass a function to a child.

Parent:

```jsx
function App() {

    function sayHello() {
        alert("Hello!");
    }

    return (
        <Child onClick={sayHello} />
    );
}
```

Child:

```jsx
function Child({ onClick }) {

    return (
        <button onClick={onClick}>
            Click Me
        </button>
    );
}
```

Flow:

```text
Parent
  ↓
Function prop
  ↓
Child
  ↓
Child calls function
  ↓
Parent function executes
```

---

# 7. Child → Parent Communication

React does not normally pass data directly from child to parent.

Instead:

```text
Parent
   ↓
Pass function
   ↓
Child
   ↓
Call function with data
   ↓
Parent receives data
```

Example:

```jsx
function Parent() {

    function receiveData(data) {

        console.log(data);
    }

    return (
        <Child sendData={receiveData} />
    );
}
```

Child:

```jsx
function Child({ sendData }) {

    return (
        <button
            onClick={() =>
                sendData("Hello Parent")
            }
        >
            Send Data
        </button>
    );
}
```

Output:

```text
Hello Parent
```

---

# 8. Child Sending Input to Parent

Parent:

```jsx
import { useState } from "react";

function Parent() {

    const [name, setName] = useState("");

    function handleName(value) {
        setName(value);
    }

    return (
        <div>

            <h2>
                Name: {name}
            </h2>

            <Child
                sendName={handleName}
            />

        </div>
    );
}
```

Child:

```jsx
function Child({ sendName }) {

    return (
        <input
            onChange={(e) =>
                sendName(e.target.value)
            }
        />
    );
}
```

Flow:

```text
Child Input
    ↓
sendName()
    ↓
Parent
    ↓
setName()
    ↓
Parent re-renders
```

---

# 9. Sibling Communication

Suppose:

```text
Parent
 ├── Child A
 └── Child B
```

Child A and Child B are siblings.

Usually they should communicate through their common parent.

```text
Child A
   ↓
Parent
   ↓
Child B
```

Example:

```text
Child A sends data
        ↓
      Parent
        ↓
Child B receives data
```

---

# 10. Example – Sibling Communication

Parent:

```jsx
function App() {

    const [message, setMessage] =
        useState("");

    return (
        <div>

            <ChildA
                sendMessage={setMessage}
            />

            <ChildB
                message={message}
            />

        </div>
    );
}
```

Child A:

```jsx
function ChildA({ sendMessage }) {

    return (
        <button
            onClick={() =>
                sendMessage("Hello from A")
            }
        >
            Send
        </button>
    );
}
```

Child B:

```jsx
function ChildB({ message }) {

    return <h2>{message}</h2>;
}
```

Flow:

```text
Child A
  ↓
Parent State
  ↓
Child B
```

---

# 11. Context API

When many components need the same data, passing props through many levels can become difficult.

This is called:

## Prop Drilling

Example:

```text
App
 ↓
Component A
 ↓
Component B
 ↓
Component C
 ↓
Component D
```

If `Component D` needs data from `App`, we may have to pass props through A, B, and C.

Context can help.

```text
Provider
   ↓
Any descendant
```

Example:

```jsx
const UserContext = createContext();
```

Provider:

```jsx
<UserContext.Provider value="Dharun">

    <Dashboard />

</UserContext.Provider>
```

Child:

```jsx
const user = useContext(UserContext);
```

---

# 12. State Lifting

**Lifting state up** means moving shared state to the nearest common parent.

Example:

```text
Before:

Child A → State
Child B → State

After:

       Parent
       State
       /   \
      ↓     ↓
   Child A Child B
```

This allows both children to use the same state.

---

# 13. Props vs Context

| Props | Context |
|---|---|
| Parent → Child | Share data with descendants |
| Explicit | Less prop passing |
| Good for local communication | Good for shared data |
| Easy to understand | Useful for avoiding prop drilling |

Use props when the relationship is simple.

Use Context when many components need the same data.

---

# 14. Component Communication Methods

Common approaches:

```text
1. Parent → Child
   Props

2. Child → Parent
   Callback function through props

3. Sibling → Sibling
   Shared parent/state

4. Deep components
   Context API

5. Shared application state
   State management solutions
```

---

# 15. Real-World Example

Consider an e-commerce application:

```text
App
 ├── Navbar
 │    └── CartCount
 │
 ├── ProductList
 │    └── ProductCard
 │
 └── Cart
```

When the user clicks:

```text
Add to Cart
```

The product information needs to reach the cart.

A simple flow can be:

```text
ProductCard
     ↓
Parent / Shared State
     ↓
Cart
```

For larger applications, shared state or Context can be useful.

---

# 16. Important Rule

React follows:

> **One-way data flow**

Normally data flows:

```text
Parent
  ↓
Child
  ↓
Grandchild
```

Not directly:

```text
Child
  ↑
Parent
```

For child-to-parent communication, the child calls a function provided by the parent.

---

# 17. Common Mistakes

### ❌ Modifying props

```jsx
props.name = "Dharun";
```

Props should not be directly modified.

### ✅ Use state in the appropriate component

```jsx
setName("Dharun");
```

---

### ❌ Passing too many unnecessary props

If many levels need the same data:

```text
A → B → C → D → E
```

Consider:

```text
Context
```

or an appropriate shared-state solution.

---

# 🎯 Interview One-Liners

### How does a parent communicate with a child?

> A parent communicates with a child by passing data through props.

### How does a child communicate with a parent?

> The parent passes a callback function as a prop, and the child calls that function with data.

### How do sibling components communicate?

> Sibling components commonly communicate through shared state in their nearest common parent.

### What is prop drilling?

> Prop drilling is passing props through intermediate components that do not need the data themselves.

### How can prop drilling be reduced?

> Context API or a suitable shared-state solution can reduce prop drilling.

### What is one-way data flow?

> In React, data normally flows from parent components to child components through props.

---

# 🧠 Quick Revision

```text
Parent → Child
     ↓
   Props

Child → Parent
     ↓
Callback function

Sibling → Sibling
     ↓
Common Parent

Deep Components
     ↓
Context API

Shared State
     ↓
Lift state up
```

# ⭐ Key Point

> **React components communicate mainly through props, callback functions, shared parent state, and Context.**
