# 📘 React – Props

## 1. What are Props?

**Props** stands for **Properties**.

Props are used to **pass data from a parent component to a child component**.

### Simple Definition

> Props are read-only data passed from a parent component to a child component.

---

# 2. Why Do We Use Props?

Props help us:

- Pass data between components
- Reuse components with different data
- Make components dynamic
- Communicate from parent to child

---

# 3. Basic Example

### Parent Component

```jsx
function App() {
    return (
        <Student name="Dharun" />
    );
}
```

### Child Component

```jsx
function Student(props) {
    return <h2>Hello {props.name}</h2>;
}
```

### Output

```text
Hello Dharun
```

Here:

```text
App
 ↓
Student
 ↓
name = "Dharun"
```

---

# 4. Passing Multiple Props

We can pass multiple values.

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

Child component:

```jsx
function Student(props) {
    return (
        <div>
            <h2>Name: {props.name}</h2>
            <p>Age: {props.age}</p>
            <p>Course: {props.course}</p>
        </div>
    );
}
```

### Output

```text
Name: Dharun
Age: 21
Course: CSE
```

---

# 5. Props Using Destructuring

Instead of:

```jsx
function Student(props) {
    return <h2>{props.name}</h2>;
}
```

We can use destructuring:

```jsx
function Student({ name }) {
    return <h2>{name}</h2>;
}
```

For multiple props:

```jsx
function Student({ name, age, course }) {
    return (
        <div>
            <h2>{name}</h2>
            <p>{age}</p>
            <p>{course}</p>
        </div>
    );
}
```

This is commonly used in React applications.

---

# 6. Props Can Have Different Data Types

Props can contain:

```text
String
Number
Boolean
Array
Object
Function
```

### Example

```jsx
function App() {

    const skills = ["Java", "React", "SQL"];

    return (
        <Student
            name="Dharun"
            age={21}
            isStudent={true}
            skills={skills}
        />
    );
}
```

---

# 7. Passing an Array

```jsx
function App() {

    const skills = ["Java", "React", "SQL"];

    return <Student skills={skills} />;
}
```

Child:

```jsx
function Student({ skills }) {

    return (
        <div>
            {skills.map(skill => (
                <p key={skill}>{skill}</p>
            ))}
        </div>
    );
}
```

---

# 8. Passing an Object

```jsx
function App() {

    const student = {
        name: "Dharun",
        age: 21
    };

    return <Student data={student} />;
}
```

Child:

```jsx
function Student({ data }) {

    return (
        <div>
            <h2>{data.name}</h2>
            <p>{data.age}</p>
        </div>
    );
}
```

---

# 9. Passing a Function

A function can also be passed as a prop.

### Parent

```jsx
function App() {

    function sayHello() {
        alert("Hello!");
    }

    return <Button onClick={sayHello} />;
}
```

### Child

```jsx
function Button({ onClick }) {

    return (
        <button onClick={onClick}>
            Click Me
        </button>
    );
}
```

This is useful when a child component needs to trigger an action defined by the parent.

---

# 10. Props are Read-Only

Props should **not be directly modified** by the child component.

### ❌ Wrong

```jsx
function Student(props) {

    props.name = "Ajay";

    return <h2>{props.name}</h2>;
}
```

### Correct Approach

If data needs to change, use **state**.

We will learn state in the upcoming topics.

---

# 11. Props vs State

| Props | State |
|---|---|
| Passed from parent | Managed inside component |
| Read-only | Can be updated |
| Used to pass data | Used to manage changing data |
| Child should not modify props | State can be updated |
| Helps communication | Helps dynamic UI |

### Easy Memory

```text
Props  → Data coming IN
State  → Data managed INSIDE
```

---

# 12. Props Flow

React normally follows **one-way data flow**.

```text
Parent
   ↓
 Props
   ↓
Child
```

Example:

```text
App
 ↓
Student
 ↓
name = "Dharun"
```

The child receives data from the parent.

---

# 13. `children` Prop

React provides a special prop called `children`.

Example:

```jsx
function Card({ children }) {

    return (
        <div className="card">
            {children}
        </div>
    );
}
```

Use it:

```jsx
function App() {

    return (
        <Card>
            <h2>Hello Dharun</h2>
            <p>Welcome to React</p>
        </Card>
    );
}
```

The content inside `<Card>...</Card>` becomes the `children` prop.

---

# 14. Real-World Example

Imagine a product card.

```jsx
function ProductCard({ name, price }) {

    return (
        <div>
            <h2>{name}</h2>
            <p>₹{price}</p>
        </div>
    );
}
```

Use it:

```jsx
function App() {

    return (
        <div>
            <ProductCard
                name="Laptop"
                price={50000}
            />

            <ProductCard
                name="Mobile"
                price={20000}
            />
        </div>
    );
}
```

The same component displays different products.

---

# 🧠 Easy Memory Trick

```text
Props
 ↓
Parent
 ↓
Pass Data
 ↓
Child
 ↓
Read Data
```

### Remember:

```text
Props = Properties
Props = Read Only
Props = Parent → Child
```

---

# 🎯 Interview One-Liners

### What are props?

> Props are read-only properties used to pass data from a parent component to a child component.

### Can we modify props?

> No. Props are read-only and should not be directly modified by the child.

### Can props contain different data types?

> Yes. Props can contain strings, numbers, booleans, arrays, objects, functions, and other values.

### What is one-way data flow?

> In React, data normally flows from parent components to child components through props.

### What is the `children` prop?

> `children` is a special prop that contains the content placed between a component's opening and closing tags.

### Props vs State?

> Props are read-only data received from a parent, while state is data managed and updated inside a component.

---

# 🔥 Quick Revision

```text
Props
→ Properties
→ Pass data from Parent → Child
→ Read-only
→ Make components dynamic
→ Can contain different data types
→ Support one-way data flow
→ Can pass functions
→ children is a special prop
```

### Example

```jsx
function App() {
    return <Student name="Dharun" age={21} />;
}

function Student({ name, age }) {
    return (
        <h2>
            {name} - {age}
        </h2>
    );
}
```

# ⭐ Key Point

> **Props allow a parent component to pass read-only data to a child component, making components reusable and dynamic.**
