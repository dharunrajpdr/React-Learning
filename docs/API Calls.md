# 📘 React – API Calls

## 1. What is an API?

**API** stands for **Application Programming Interface**.

In a React application, an API is commonly used to communicate with a backend server.

Example:

```text
React Frontend
      ↓
    API Call
      ↓
Node.js / Express Backend
      ↓
   Database
```

---

# 2. Why Do We Need API Calls?

React handles the **frontend/UI**.

The backend usually handles:

- Database operations
- Authentication
- Business logic
- Data processing
- Sending/receiving data

React needs API calls to get or send this data.

Example:

```text
Login Form
    ↓
POST /api/login
    ↓
Backend
    ↓
Database
    ↓
Response
    ↓
React UI
```

---

# 3. Common HTTP Methods

| Method | Purpose |
|---|---|
| GET | Get data |
| POST | Create/send data |
| PUT | Update data |
| PATCH | Partially update data |
| DELETE | Delete data |

Example:

```text
GET    /api/users
POST   /api/users
PUT    /api/users/10
DELETE /api/users/10
```

---

# 4. Fetch API

JavaScript provides a built-in API called `fetch()`.

Basic syntax:

```jsx
fetch("https://example.com/api/users")
```

It returns a Promise.

---

# 5. GET Request

Example:

```jsx
fetch("https://example.com/api/users")
    .then(response => response.json())
    .then(data => {
        console.log(data);
    })
    .catch(error => {
        console.log(error);
    });
```

Flow:

```text
fetch()
 ↓
Server
 ↓
Response
 ↓
response.json()
 ↓
Data
```

---

# 6. API Call with `async/await`

`async/await` makes asynchronous code easier to read.

```jsx
async function getUsers() {

    try {

        const response =
            await fetch("https://example.com/api/users");

        const data =
            await response.json();

        console.log(data);

    } catch (error) {

        console.log(error);

    }
}
```

---

# 7. API Call Inside `useEffect`

When data needs to be fetched when a component loads, `useEffect` is commonly used.

```jsx
import { useEffect } from "react";

function Users() {

    useEffect(() => {

        async function getUsers() {

            const response =
                await fetch("https://example.com/api/users");

            const data =
                await response.json();

            console.log(data);
        }

        getUsers();

    }, []);

    return <h1>Users</h1>;
}
```

Because the dependency array is:

```jsx
[]
```

the effect runs after the initial render.

---

# 8. Storing API Data in State

Usually we don't just `console.log()` the data.

We store it in state.

```jsx
import { useEffect, useState } from "react";

function Users() {

    const [users, setUsers] = useState([]);

    useEffect(() => {

        async function getUsers() {

            const response =
                await fetch("https://example.com/api/users");

            const data =
                await response.json();

            setUsers(data);
        }

        getUsers();

    }, []);

    return (
        <div>

            {users.map(user => (
                <p key={user.id}>
                    {user.name}
                </p>
            ))}

        </div>
    );
}
```

Flow:

```text
API
 ↓
fetch()
 ↓
response.json()
 ↓
setUsers()
 ↓
State updated
 ↓
Component re-renders
 ↓
Users displayed
```

---

# 9. Loading State

API calls take some time.

So we can show:

```text
Loading...
```

Example:

```jsx
const [loading, setLoading] = useState(true);
```

Complete example:

```jsx
import {
    useEffect,
    useState
} from "react";

function Users() {

    const [users, setUsers] = useState([]);
    const [loading, setLoading] = useState(true);

    useEffect(() => {

        async function getUsers() {

            try {

                const response =
                    await fetch(
                        "https://example.com/api/users"
                    );

                const data =
                    await response.json();

                setUsers(data);

            } finally {

                setLoading(false);
            }
        }

        getUsers();

    }, []);

    if (loading) {
        return <h2>Loading...</h2>;
    }

    return (
        <div>

            {users.map(user => (
                <p key={user.id}>
                    {user.name}
                </p>
            ))}

        </div>
    );
}
```

---

# 10. Error Handling

API requests can fail.

Examples:

```text
Server error
Network error
Invalid URL
Unauthorized request
Database error
```

Use `try/catch`.

```jsx
const [error, setError] = useState("");

try {

    const response =
        await fetch(url);

    if (!response.ok) {
        throw new Error("Failed to fetch data");
    }

    const data =
        await response.json();

    setUsers(data);

} catch (error) {

    setError(error.message);

}
```

---

# 11. `response.ok`

Important:

`fetch()` does **not automatically throw an error for HTTP errors such as 404 or 500**.

So we can check:

```jsx
if (!response.ok) {
    throw new Error("Request failed");
}
```

Example:

```jsx
const response = await fetch(url);

if (!response.ok) {
    throw new Error("Failed to fetch users");
}
```

---

# 12. Complete GET Example

```jsx
import {
    useEffect,
    useState
} from "react";

function Users() {

    const [users, setUsers] = useState([]);
    const [loading, setLoading] = useState(true);
    const [error, setError] = useState("");

    useEffect(() => {

        async function getUsers() {

            try {

                const response =
                    await fetch(
                        "https://example.com/api/users"
                    );

                if (!response.ok) {
                    throw new Error(
                        "Failed to fetch users"
                    );
                }

                const data =
                    await response.json();

                setUsers(data);

            } catch (error) {

                setError(error.message);

            } finally {

                setLoading(false);

            }
        }

        getUsers();

    }, []);

    if (loading) {
        return <h2>Loading...</h2>;
    }

    if (error) {
        return <h2>{error}</h2>;
    }

    return (
        <div>

            <h1>Users</h1>

            {users.map(user => (
                <p key={user.id}>
                    {user.name}
                </p>
            ))}

        </div>
    );
}
```

---

# 13. POST Request

POST is commonly used to send data to the server.

Example:

```jsx
async function createUser() {

    const response = await fetch(
        "https://example.com/api/users",
        {
            method: "POST",

            headers: {
                "Content-Type": "application/json"
            },

            body: JSON.stringify({
                name: "Dharun",
                age: 21
            })
        }
    );

    const data =
        await response.json();

    console.log(data);
}
```

---

# 14. Understanding POST

```jsx
method: "POST"
```

specifies the HTTP method.

```jsx
headers: {
    "Content-Type": "application/json"
}
```

tells the server that we are sending JSON.

```jsx
body: JSON.stringify({
    name: "Dharun",
    age: 21
})
```

converts the JavaScript object into JSON text.

---

# 15. JSON

JSON is commonly used for exchanging data between frontend and backend.

Example:

```json
{
    "name": "Dharun",
    "age": 21,
    "role": "Developer"
}
```

Convert JavaScript object to JSON:

```jsx
JSON.stringify(data)
```

Convert JSON response to JavaScript object:

```jsx
response.json()
```

Remember:

```text
JavaScript Object
      ↓
JSON.stringify()
      ↓
JSON

JSON
 ↓
response.json()
 ↓
JavaScript Object
```

---

# 16. PUT Request

PUT is commonly used to update data.

```jsx
await fetch(
    "https://example.com/api/users/10",
    {
        method: "PUT",

        headers: {
            "Content-Type": "application/json"
        },

        body: JSON.stringify({
            name: "Dharun Raj"
        })
    }
);
```

---

# 17. DELETE Request

DELETE is used to delete data.

```jsx
await fetch(
    "https://example.com/api/users/10",
    {
        method: "DELETE"
    }
);
```

---

# 18. API Call Flow in React

```text
Component
    ↓
useEffect / Event Handler
    ↓
fetch()
    ↓
Backend API
    ↓
Server Processing
    ↓
Database
    ↓
Response
    ↓
setState()
    ↓
UI Update
```

---

# 19. GET vs POST

| GET | POST |
|---|---|
| Retrieve data | Send/create data |
| Usually no request body | Usually has request body |
| Example: Get users | Example: Create user |

Example:

```text
GET /api/users
```

```text
POST /api/users
```

---

# 20. Real MERN Example

Suppose your Express backend has:

```js
app.get("/api/users", getUsers);
```

React can call:

```jsx
const response =
    await fetch("http://localhost:5000/api/users");

const users =
    await response.json();
```

Flow:

```text
React
 ↓
GET /api/users
 ↓
Express
 ↓
MongoDB
 ↓
Users data
 ↓
React
```

---

# 21. API Calls and `useEffect`

A common pattern is:

```jsx
useEffect(() => {

    async function fetchData() {

        try {

            const response = await fetch(url);

            const data = await response.json();

            setData(data);

        } catch (error) {

            setError(error.message);

        }

    }

    fetchData();

}, []);
```

Remember:

> **useEffect → API call → setState → UI update**

---

# 22. Common Mistakes

### ❌ Don't directly make the effect callback async

Avoid:

```jsx
useEffect(async () => {
    // ...
}, []);
```

### ✅ Better

```jsx
useEffect(() => {

    async function fetchData() {
        // API call
    }

    fetchData();

}, []);
```

---

### ❌ Forgetting `response.json()`

```jsx
const data = response;
```

### ✅ Correct

```jsx
const data = await response.json();
```

---

### ❌ Forgetting error handling

Always consider:

```jsx
try {
    // API call
} catch (error) {
    // Handle error
}
```

---

# 23. Important API States

A good API-based component commonly handles:

```text
Loading
   ↓
Success
   ↓
Data

OR

Loading
   ↓
Error
   ↓
Error Message
```

So commonly we use:

```jsx
const [data, setData] = useState([]);
const [loading, setLoading] = useState(true);
const [error, setError] = useState("");
```

---

# 🎯 Interview One-Liners

### What is an API?

> An API allows different software components to communicate with each other.

### How do you call an API in React?

> We can use the browser's `fetch()` API or a library such as Axios to make HTTP requests.

### Why is `useEffect` commonly used for API calls?

> `useEffect` is commonly used to perform side effects such as fetching data after a component renders.

### What is `response.json()`?

> It reads the response body and parses JSON data into a JavaScript value.

### What is `JSON.stringify()`?

> It converts a JavaScript value into a JSON string.

### Why use loading state?

> Loading state lets us show the user that an asynchronous operation is in progress.

### Why use error state?

> Error state allows the UI to display a meaningful message when an API request fails.

---

# 🧠 Quick Revision

```text
API
→ Communication between frontend and backend

GET
→ Get data

POST
→ Create/send data

PUT
→ Update data

PATCH
→ Partially update data

DELETE
→ Delete data

fetch()
→ Make HTTP request

response.json()
→ Convert JSON response to JS value

JSON.stringify()
→ Convert JS value to JSON string

useEffect
→ Commonly used for fetching data on render

Loading
→ Request is in progress

Error
→ Request failed

Success
→ Data received
```

# ⭐ Key Point

> **React uses API calls to communicate with the backend and retrieve or send data. `fetch()` is built into JavaScript, while libraries like Axios provide another way to make HTTP requests.**
