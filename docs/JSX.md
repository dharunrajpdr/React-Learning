# 📘 React – JSX

## 1. What is JSX?

**JSX** stands for **JavaScript XML**.

JSX allows us to write **HTML-like syntax inside JavaScript**.

### Simple Definition

> JSX is a syntax extension of JavaScript that allows us to write HTML-like code inside JavaScript.

---

# 2. Why Do We Use JSX?

Without JSX, creating UI can be more difficult to read.

### Without JSX

```javascript
const element = React.createElement(
    "h1",
    null,
    "Hello React"
);
```

### With JSX

```jsx
const element = <h1>Hello React</h1>;
```

JSX makes React code **cleaner and easier to understand**.

---

# 3. Simple JSX Example

```jsx
function App() {
    return <h1>Hello React</h1>;
}

export default App;
```

### Output

```text
Hello React
```

---

# 4. JSX with Multiple Elements

A component must return a single parent structure.

### Example

```jsx
function App() {
    return (
        <div>
            <h1>Hello</h1>
            <p>Welcome to React</p>
        </div>
    );
}
```

Here, `<div>` is the parent element.

---

# 5. JSX Expressions

We can write JavaScript expressions inside JSX using:

```text
{ }
```

Example:

```jsx
function App() {

    const name = "Dharun";

    return (
        <h1>Hello {name}</h1>
    );
}
```

### Output

```text
Hello Dharun
```

---

# 6. JavaScript Expressions in JSX

We can use expressions such as:

```jsx
<h1>{10 + 20}</h1>
```

Output:

```text
30
```

We can also use:

```jsx
<h1>{name.toUpperCase()}</h1>
```

---

# 7. JSX with Variables

```jsx
function App() {

    const name = "Dharun";
    const age = 21;

    return (
        <div>
            <h1>Name: {name}</h1>
            <p>Age: {age}</p>
        </div>
    );
}
```

### Output

```text
Name: Dharun
Age: 21
```

---

# 8. JSX Attributes

JSX uses attributes similar to HTML.

Example:

```jsx
<img src="image.jpg" alt="Profile" />
```

But some attribute names are different from HTML.

### Example

HTML:

```html
<div class="container"></div>
```

JSX:

```jsx
<div className="container"></div>
```

### Common JSX attributes

```text
class      → className
for        → htmlFor
onclick    → onClick
```

---

# 9. JSX Uses camelCase

Many JSX attributes use **camelCase**.

Example:

```jsx
<button onClick={handleClick}>
    Click
</button>
```

Not:

```jsx
<button onclick={handleClick}>
```

---

# 10. JSX Comments

Comments inside JSX are written like:

```jsx
function App() {

    return (
        <div>
            {/* This is a JSX comment */}
            <h1>Hello React</h1>
        </div>
    );
}
```

---

# 11. JSX Must Be Properly Closed

HTML sometimes allows certain tags without closing tags.

In JSX, elements should be properly closed.

### Correct

```jsx
<img src="image.jpg" />
```

```jsx
<input type="text" />
```

### Incorrect

```jsx
<img src="image.jpg">
```

---

# 12. JSX and JavaScript

JSX is **not HTML**.

It looks like HTML, but it is written inside JavaScript and is transformed into JavaScript that React can use.

Example:

```jsx
const element = <h1>Hello</h1>;
```

Conceptually, it is transformed into a React element creation call.

---

# 13. JSX Can Use JavaScript Logic

Example:

```jsx
function App() {

    const age = 21;

    return (
        <div>
            {age >= 18 && <p>Adult</p>}
        </div>
    );
}
```

Output:

```text
Adult
```

We will learn conditional rendering in detail later.

---

# 14. JSX with Function Calls

```jsx
function getName() {
    return "Dharun";
}

function App() {
    return <h1>Hello {getName()}</h1>;
}
```

### Output

```text
Hello Dharun
```

---

# 15. JSX Rules

### Rule 1: Return one parent element

```jsx
return (
    <div>
        <h1>Hello</h1>
        <p>Welcome</p>
    </div>
);
```

---

### Rule 2: Close all elements

```jsx
<input />
```

---

### Rule 3: Use `className`

```jsx
<div className="container">
```

---

### Rule 4: Use `{}` for JavaScript expressions

```jsx
<h1>{name}</h1>
```

---

### Rule 5: Use camelCase for many attributes

```jsx
onClick
className
tabIndex
```

---

# 16. JSX vs HTML

| HTML | JSX |
|---|---|
| `class` | `className` |
| `for` | `htmlFor` |
| `onclick` | `onClick` |
| Can use plain HTML | Can include JavaScript expressions |
| Comments use `<!-- -->` | Comments use `{/* */}` |
| JSX elements should be properly closed | Yes |

---

# 🧠 Easy Memory Trick

```text
JSX
 ↓
JavaScript + HTML-like Syntax
 ↓
{ } → JavaScript Expressions
 ↓
className → CSS Class
 ↓
camelCase → JSX Attributes
```

---

# 🎯 Interview One-Liners

### What is JSX?

> JSX is a syntax extension of JavaScript that allows us to write HTML-like syntax inside JavaScript.

### Is JSX HTML?

> No. JSX looks like HTML but is a syntax extension of JavaScript used by React.

### Why do we use JSX?

> JSX makes React UI code easier to write and understand.

### How do you write JavaScript inside JSX?

> We use curly braces `{}` to embed JavaScript expressions.

### Why do we use `className` instead of `class`?

> Because `class` is a JavaScript keyword, so JSX uses `className` for CSS classes.

---

# 🔥 Quick Revision

```text
JSX
→ JavaScript XML
→ HTML-like syntax
→ Used to describe UI
→ JavaScript expressions use { }
→ className instead of class
→ camelCase attributes
→ Elements should be properly closed
→ Makes React UI code easier to read
```

# ⭐ Key Point

> **JSX allows us to write HTML-like UI syntax directly inside JavaScript, making React components easier to create and understand.**
