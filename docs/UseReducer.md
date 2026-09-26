# 📘 React – useReducer Hook

## 1. What is `useReducer`?

`useReducer` is a React Hook used to manage **complex state logic**.

### Simple Definition

> `useReducer` manages state using a reducer function and actions.

It is especially useful when:

```text
Multiple state updates
Complex state logic
Many related actions
State depends on previous state
```

---

# 2. Basic Syntax

```jsx
const [state, dispatch] = useReducer(reducer, initialState);
```

Here:

```text
state
→ Current state

dispatch
→ Sends an action

reducer
→ Decides how state should change

initialState
→ Starting state
```

---

# 3. Import `useReducer`

```jsx
import { useReducer } from "react";
```

---

# 4. Reducer Function

A reducer is a function that receives:

```text
Current state
+
Action
```

and returns:

```text
New state
```

Example:

```jsx
function reducer(state, action) {

    if (action.type === "increment") {
        return state + 1;
    }

    return state;
}
```

---

# 5. Simple Counter Example

```jsx
import { useReducer } from "react";

function reducer(state, action) {

    if (action.type === "increment") {
        return state + 1;
    }

    if (action.type === "decrement") {
        return state - 1;
    }

    return state;
}

function App() {

    const [count, dispatch] = useReducer(
        reducer,
        0
    );

    return (
        <div>

            <h1>{count}</h1>

            <button
                onClick={() =>
                    dispatch({ type: "increment" })
                }
            >
                +
            </button>

            <button
                onClick={() =>
                    dispatch({ type: "decrement" })
                }
            >
                -
            </button>

        </div>
    );
}
```

---

# 6. Understanding the Flow

When we click:

```jsx
dispatch({ type: "increment" });
```

The flow is:

```text
Button Click
     ↓
dispatch()
     ↓
Action
     ↓
reducer()
     ↓
New State
     ↓
Component Re-renders
```

---

# 7. What is an Action?

An action describes **what happened**.

Example:

```jsx
{
    type: "increment"
}
```

Another:

```jsx
{
    type: "decrement"
}
```

The `type` usually describes the operation.

---

# 8. Action with Data

An action can contain additional data.

Example:

```jsx
dispatch({
    type: "add",
    amount: 5
});
```

Reducer:

```jsx
function reducer(state, action) {

    if (action.type === "add") {
        return state + action.amount;
    }

    return state;
}
```

Now:

```text
Current state = 10
amount = 5

New state = 15
```

---

# 9. Using `switch`

A `switch` statement is commonly used in reducers.

```jsx
function reducer(state, action) {

    switch (action.type) {

        case "increment":
            return state + 1;

        case "decrement":
            return state - 1;

        default:
            return state;
    }
}
```

This is often easier to read when there are many actions.

---

# 10. `useReducer` with Object State

A more realistic example:

```jsx
import { useReducer } from "react";

const initialState = {
    name: "",
    age: 0
};

function reducer(state, action) {

    switch (action.type) {

        case "setName":
            return {
                ...state,
                name: action.payload
            };

        case "setAge":
            return {
                ...state,
                age: action.payload
            };

        default:
            return state;
    }
}

function App() {

    const [state, dispatch] = useReducer(
        reducer,
        initialState
    );

    return (
        <div>

            <h2>{state.name}</h2>
            <p>{state.age}</p>

            <button
                onClick={() =>
                    dispatch({
                        type: "setName",
                        payload: "Dharun"
                    })
                }
            >
                Set Name
            </button>

        </div>
    );
}
```

---

# 11. What is `payload`?

`payload` is commonly used to carry data with an action.

Example:

```jsx
dispatch({
    type: "setName",
    payload: "Dharun"
});
```

Here:

```text
type
→ setName

payload
→ Dharun
```

Then:

```jsx
action.payload
```

gets:

```text
Dharun
```

---

# 12. `useReducer` vs `useState`

| `useState` | `useReducer` |
|---|---|
| Simple state logic | Complex state logic |
| Easy to use | More structured |
| Direct setter | Uses `dispatch()` |
| Good for simple values | Good for multiple related actions |
| Example: toggle/count | Example: form/cart state |

### Example

Simple:

```jsx
const [count, setCount] = useState(0);
```

Complex:

```jsx
const [state, dispatch] = useReducer(
    reducer,
    initialState
);
```

---

# 13. When Should We Use `useReducer`?

Use `useReducer` when:

```text
State has many related values
        OR
Many actions can change the state
        OR
State update logic is complex
```

Example:

```text
Shopping Cart

ADD_ITEM
REMOVE_ITEM
INCREASE_QUANTITY
DECREASE_QUANTITY
CLEAR_CART
```

This can become easier to manage with a reducer.

---

# 14. Example – Shopping Cart Concept

```jsx
const initialState = {
    cart: []
};

function reducer(state, action) {

    switch (action.type) {

        case "ADD_ITEM":
            return {
                ...state,
                cart: [...state.cart, action.payload]
            };

        case "CLEAR_CART":
            return {
                ...state,
                cart: []
            };

        default:
            return state;
    }
}
```

Dispatch:

```jsx
dispatch({
    type: "ADD_ITEM",
    payload: {
        id: 1,
        name: "Laptop"
    }
});
```

---

# 15. Reducer Should Be Predictable

A reducer should generally:

```text
Receive state
+
Receive action
↓
Return new state
```

Avoid performing unrelated side effects inside the reducer.

For example, API calls should not normally be placed inside the reducer.

---

# 16. Don't Directly Mutate State

### ❌ Wrong

```jsx
state.name = "Dharun";

return state;
```

### ✅ Correct

```jsx
return {
    ...state,
    name: "Dharun"
};
```

Create and return a new state object rather than directly modifying the existing state.

---

# 17. Important Terms

### Reducer

Function that determines the next state.

```jsx
function reducer(state, action) {
    // ...
}
```

### Action

Describes what happened.

```jsx
{
    type: "increment"
}
```

### Dispatch

Sends an action.

```jsx
dispatch({
    type: "increment"
});
```

### Initial State

Starting state.

```jsx
const initialState = 0;
```

---

# 🧠 Easy Memory Trick

Remember:

```text
Action
  ↓
dispatch()
  ↓
Reducer
  ↓
New State
  ↓
UI Update
```

Or simply:

> **Action → Dispatch → Reducer → State**

---

# 🎯 Interview One-Liners

### What is `useReducer`?

> `useReducer` is a React Hook used to manage state with a reducer function and actions, especially when state logic is complex.

### What is a reducer?

> A reducer is a function that takes the current state and an action and returns the new state.

### What is `dispatch`?

> `dispatch` is used to send an action to the reducer.

### What is an action?

> An action is an object that describes what happened and usually contains a `type`.

### What is a payload?

> A payload contains additional data required to perform a state update.

### `useState` vs `useReducer`?

> `useState` is generally convenient for simple state, while `useReducer` is useful when state logic involves multiple related actions or becomes more complex.

---

# 🔥 Quick Revision

```text
useReducer
→ Manage complex state

Syntax:

const [state, dispatch] =
    useReducer(reducer, initialState);

reducer
→ Calculates new state

action
→ Describes what happened

dispatch()
→ Sends action

payload
→ Additional data

Flow:

Action
 ↓
dispatch()
 ↓
reducer()
 ↓
New State
 ↓
UI Update
```

# ⭐ Key Point

> **Use `useReducer` when state management becomes complex and you want to organize state updates through actions and a reducer function.**
