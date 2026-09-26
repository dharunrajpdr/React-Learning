# 📘 React – Lists & Keys

## 1. What are Lists in React?

A **list** means displaying multiple items from an array or collection.

For example:

```text
Java
React
Node.js
MongoDB
```

Instead of writing every item manually, React can generate the UI from an array.

---

# 2. Rendering a List with `map()`

The most common way to render lists in React is using JavaScript's `map()` method.

```jsx
function App() {

    const skills = ["Java", "React", "Node.js", "MongoDB"];

    return (
        <div>
            {skills.map((skill) => (
                <p>{skill}</p>
            ))}
        </div>
    );
}
```

### Output

```text
Java
React
Node.js
MongoDB
```

---

# 3. Why use `map()`?

Suppose we have:

```js
const skills = ["Java", "React", "Node.js"];
```

Instead of:

```jsx
<p>Java</p>
<p>React</p>
<p>Node.js</p>
```

We can use:

```jsx
{skills.map((skill) => (
    <p>{skill}</p>
))}
```

This makes the code:

```text
Reusable
Dynamic
Shorter
Easier to maintain
```

---

# 4. What is a Key?

A **key** is a special prop used by React to uniquely identify each item in a list.

Example:

```jsx
function App() {

    const skills = ["Java", "React", "Node.js"];

    return (
        <div>
            {skills.map((skill) => (
                <p key={skill}>{skill}</p>
            ))}
        </div>
    );
}
```

Here:

```jsx
key={skill}
```

gives each element a unique identity.

---

# 5. Why are Keys Important?

React uses keys to identify which list items have:

```text
Added
Removed
Changed
Moved
```

This helps React update the UI correctly.

### Simple idea

```text
Array
 ↓
React compares keys
 ↓
Identifies changed items
 ↓
Updates required UI
```

---

# 6. Key Should Be Unique

Keys should be unique among the items in the list.

Example:

```jsx
const users = [
    { id: 1, name: "Dharun" },
    { id: 2, name: "Ajay" },
    { id: 3, name: "Rahul" }
];
```

Use:

```jsx
{users.map((user) => (
    <p key={user.id}>
        {user.name}
    </p>
))}
```

Here:

```text
1 → Dharun
2 → Ajay
3 → Rahul
```

---

# 7. Rendering Objects

A common real-world example:

```jsx
function App() {

    const users = [
        { id: 1, name: "Dharun", age: 21 },
        { id: 2, name: "Ajay", age: 22 },
        { id: 3, name: "Rahul", age: 21 }
    ];

    return (
        <div>

            {users.map((user) => (
                <div key={user.id}>
                    <h2>{user.name}</h2>
                    <p>Age: {user.age}</p>
                </div>
            ))}

        </div>
    );
}
```

---

# 8. Using `index` as Key

We can technically use the array index:

```jsx
{skills.map((skill, index) => (
    <p key={index}>
        {skill}
    </p>
))}
```

Here:

```text
skill → Current item
index → Position
```

Example:

```text
Java     → 0
React    → 1
Node.js  → 2
```

---

# 9. Should We Use Index as Key?

### Prefer:

```jsx
key={item.id}
```

### Avoid when possible:

```jsx
key={index}
```

Using index can cause problems when list items are:

```text
Added
Removed
Reordered
```

For a static list that never changes, using the index may be acceptable.

---

# 10. What Happens Without a Key?

Example:

```jsx
{skills.map((skill) => (
    <p>{skill}</p>
))}
```

React will show a warning similar to:

```text
Each child in a list should have a unique "key" prop.
```

So we should provide a key.

---

# 11. Key is Not Available as a Normal Prop

Important:

```jsx
<User key={user.id} />
```

Inside `User`, you cannot access:

```jsx
props.key
```

`key` is a special React prop.

If the component needs the ID, pass it separately:

```jsx
<User
    key={user.id}
    id={user.id}
/>
```

Then:

```jsx
function User({ id }) {
    return <p>{id}</p>;
}
```

---

# 12. Rendering Components in a List

We can render components using `map()`.

### User Component

```jsx
function User({ name }) {
    return <h2>{name}</h2>;
}
```

### App Component

```jsx
function App() {

    const users = [
        { id: 1, name: "Dharun" },
        { id: 2, name: "Ajay" },
        { id: 3, name: "Rahul" }
    ];

    return (
        <div>

            {users.map((user) => (
                <User
                    key={user.id}
                    name={user.name}
                />
            ))}

        </div>
    );
}
```

---

# 13. List with Conditional Rendering

We can combine lists with conditions.

```jsx
function App() {

    const users = [];

    return (
        <div>

            {users.length > 0 ? (
                users.map((user) => (
                    <p key={user.id}>
                        {user.name}
                    </p>
                ))
            ) : (
                <p>No users found</p>
            )}

        </div>
    );
}
```

---

# 14. List Example – Products

```jsx
function App() {

    const products = [
        { id: 1, name: "Laptop", price: 50000 },
        { id: 2, name: "Phone", price: 20000 },
        { id: 3, name: "Mouse", price: 1000 }
    ];

    return (
        <div>

            {products.map((product) => (
                <div key={product.id}>

                    <h2>{product.name}</h2>

                    <p>
                        Price: ₹{product.price}
                    </p>

                </div>
            ))}

        </div>
    );
}
```

---

# 15. `map()` Important Syntax

```jsx
array.map((item) => (
    <Element key={uniqueValue}>
        {item}
    </Element>
))
```

Example:

```jsx
skills.map((skill) => (
    <li key={skill}>
        {skill}
    </li>
))
```

---

# 🧠 Easy Memory Trick

```text
Array
  ↓
map()
  ↓
Create UI
  ↓
Give unique key
  ↓
React identifies each item
```

Remember:

> **map = create list, key = identify list item**

---

# 🎯 Interview One-Liners

### What is `map()` used for in React?

> `map()` is commonly used to transform an array into a list of React elements.

### What is a key in React?

> A key is a unique identifier that helps React identify list items between renders.

### Why are keys important?

> Keys help React efficiently determine which list items were added, removed, changed, or reordered.

### Can we use array index as a key?

> Yes, but it should generally be avoided for dynamic lists where items can be added, removed, or reordered.

### Should keys be unique?

> Yes, keys should be unique among sibling elements in a list.

### Can we access `key` through props?

> No. `key` is a special React prop and is not passed to the component as a normal prop.

---

# 🔥 Quick Revision

```text
Lists
→ Display multiple items

map()
→ Generate UI from an array

Key
→ Unique identity for each list item

Preferred:
key={item.id}

Avoid when possible:
key={index}
```

### Basic Example

```jsx
const skills = ["Java", "React", "Node.js"];

return (
    <ul>
        {skills.map((skill) => (
            <li key={skill}>
                {skill}
            </li>
        ))}
    </ul>
);
```

# ⭐ Key Point

> **Use `map()` to render lists in React and give each list item a stable, unique `key` so React can efficiently track it.**
