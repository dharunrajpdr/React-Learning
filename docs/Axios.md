# 📘 React – Axios

## 1. What is Axios?

**Axios** is a JavaScript library used to make HTTP requests from frontend applications.

It is commonly used in React applications to communicate with backend APIs.

Example:

```text
React
  ↓
Axios
  ↓
Express API
  ↓
Database
```

---

# 2. Why Use Axios?

We can make API calls using both:

```text
fetch()
Axios
```

Axios provides a convenient API and handles common request/response tasks.

Common features:

- GET requests
- POST requests
- PUT requests
- PATCH requests
- DELETE requests
- Request headers
- Request/response interceptors
- Automatic JSON handling
- Error handling
- Request configuration

---

# 3. Installing Axios

Run:

```bash
npm install axios
```

Then import it:

```jsx
import axios from "axios";
```

---

# 4. Axios GET Request

Basic example:

```jsx
import axios from "axios";

async function getUsers() {

    const response =
        await axios.get(
            "https://example.com/api/users"
        );

    console.log(response.data);
}
```

Notice:

```jsx
response.data
```

Axios provides the response data directly.

---

# 5. Axios with `useEffect`

API calls are commonly made inside `useEffect`.

```jsx
import {
    useEffect,
    useState
} from "react";

import axios from "axios";

function Users() {

    const [users, setUsers] = useState([]);

    useEffect(() => {

        async function getUsers() {

            const response =
                await axios.get(
                    "https://example.com/api/users"
                );

            setUsers(response.data);
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

---

# 6. Axios Response

Axios response commonly contains:

```jsx
response.data
response.status
response.headers
response.config
```

Example:

```jsx
const response = await axios.get(url);

console.log(response.data);
console.log(response.status);
```

Most commonly, we use:

```jsx
response.data
```

---

# 7. Axios POST Request

POST is used to send/create data.

```jsx
async function createUser() {

    const response = await axios.post(
        "https://example.com/api/users",
        {
            name: "Dharun",
            age: 21
        }
    );

    console.log(response.data);
}
```

Axios automatically handles JSON request data in common cases.

---

# 8. Axios PUT Request

PUT is commonly used to update data.

```jsx
async function updateUser() {

    const response = await axios.put(
        "https://example.com/api/users/10",
        {
            name: "Dharun Raj"
        }
    );

    console.log(response.data);
}
```

---

# 9. Axios PATCH Request

PATCH is commonly used for a partial update.

```jsx
await axios.patch(
    "https://example.com/api/users/10",
    {
        age: 22
    }
);
```

---

# 10. Axios DELETE Request

```jsx
async function deleteUser(id) {

    const response =
        await axios.delete(
            `https://example.com/api/users/${id}`
        );

    console.log(response.data);
}
```

---

# 11. Axios Error Handling

Use `try-catch`.

```jsx
async function getUsers() {

    try {

        const response =
            await axios.get(
                "https://example.com/api/users"
            );

        console.log(response.data);

    } catch (error) {

        console.log(error);

    }
}
```

---

# 12. Error Message

You can inspect:

```jsx
error.message
```

Example:

```jsx
catch (error) {

    console.log(error.message);

}
```

Axios errors can also contain:

```jsx
error.response
error.request
error.config
```

For example:

```jsx
catch (error) {

    if (error.response) {

        console.log(
            error.response.status
        );

    }

}
```

---

# 13. Loading + Error + Data

A common React pattern:

```jsx
import {
    useEffect,
    useState
} from "react";

import axios from "axios";

function Users() {

    const [users, setUsers] = useState([]);
    const [loading, setLoading] = useState(true);
    const [error, setError] = useState("");

    useEffect(() => {

        async function getUsers() {

            try {

                const response =
                    await axios.get(
                        "https://example.com/api/users"
                    );

                setUsers(response.data);

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

# 14. Axios vs Fetch

| Axios | Fetch |
|---|---|
| External library | Built into modern browsers |
| `axios.get(url)` | `fetch(url)` |
| Response data: `response.data` | Need `await response.json()` |
| Rejects non-2xx responses by default | Does not reject just because status is 4xx/5xx |
| Supports interceptors | No built-in Axios-style interceptors |
| Convenient configuration | Native browser API |

---

# 15. Fetch Example

```jsx
const response = await fetch(url);

if (!response.ok) {
    throw new Error("Request failed");
}

const data = await response.json();
```

Axios:

```jsx
const response = await axios.get(url);

const data = response.data;
```

So Axios often requires less code for common API requests.

---

# 16. Axios Instance

In a real MERN project, we often create a reusable Axios instance.

Example:

```jsx
import axios from "axios";

const api = axios.create({
    baseURL: "http://localhost:5000/api"
});

export default api;
```

Then:

```jsx
import api from "./api";

const response =
    await api.get("/users");
```

Instead of:

```jsx
await axios.get(
    "http://localhost:5000/api/users"
);
```

---

# 17. Why Use Axios Instance?

Suppose our backend URL is:

```text
http://localhost:5000/api
```

Instead of repeating it:

```jsx
axios.get(
    "http://localhost:5000/api/users"
);

axios.get(
    "http://localhost:5000/api/products"
);

axios.get(
    "http://localhost:5000/api/orders"
);
```

We can configure:

```jsx
const api = axios.create({
    baseURL: "http://localhost:5000/api"
});
```

Then:

```jsx
api.get("/users");

api.get("/products");

api.get("/orders");
```

This makes the code cleaner.

---

# 18. Request Headers

We can send headers with a request.

```jsx
const response = await axios.get(
    "/users",
    {
        headers: {
            Authorization: "Bearer TOKEN"
        }
    }
);
```

Headers are commonly used for:

```text
Authentication
Content type
Custom request information
```

---

# 19. POST with Headers

```jsx
await axios.post(
    "/users",
    {
        name: "Dharun"
    },
    {
        headers: {
            Authorization: "Bearer TOKEN"
        }
    }
);
```

---

# 20. Axios Interceptors

Interceptors allow us to run code before a request or before a response is handled.

Two common types:

```text
Request Interceptor
Response Interceptor
```

---

# 21. Request Interceptor

Example:

```jsx
api.interceptors.request.use(
    (config) => {

        console.log("Request sent");

        return config;
    }
);
```

Flow:

```text
Component
    ↓
Axios Request
    ↓
Request Interceptor
    ↓
Backend
```

A request interceptor can be used for common request configuration such as attaching authentication information.

---

# 22. Response Interceptor

```jsx
api.interceptors.response.use(
    (response) => {

        return response;

    },
    (error) => {

        return Promise.reject(error);

    }
);
```

Flow:

```text
Backend
   ↓
Response
   ↓
Response Interceptor
   ↓
Component
```

---

# 23. Axios in MERN

Example backend:

```js
app.get("/api/users", getUsers);
```

Frontend:

```jsx
const response =
    await api.get("/users");

setUsers(response.data);
```

Flow:

```text
React
 ↓
Axios
 ↓
GET /api/users
 ↓
Express
 ↓
MongoDB
 ↓
Response
 ↓
Axios
 ↓
React State
 ↓
UI
```

---

# 24. Axios with Environment Variables

Instead of hardcoding:

```jsx
const api = axios.create({
    baseURL: "http://localhost:5000/api"
});
```

we can use an environment variable.

For Vite:

```env
VITE_API_URL=http://localhost:5000/api
```

Then:

```jsx
const api = axios.create({
    baseURL: import.meta.env.VITE_API_URL
});
```

This makes it easier to use different URLs for development and deployment.

---

# 25. Common Mistakes

### ❌ Forgetting `response.data`

```jsx
const response = await axios.get(url);

setUsers(response);
```

Usually you want:

```jsx
setUsers(response.data);
```

---

### ❌ Not handling errors

```jsx
const response = await axios.get(url);
```

Better:

```jsx
try {

    const response =
        await axios.get(url);

} catch (error) {

    console.log(error);

}
```

---

### ❌ Repeating the base URL

Instead of repeatedly writing:

```jsx
http://localhost:5000/api
```

create:

```jsx
axios.create({
    baseURL: "http://localhost:5000/api"
});
```

---

# 🎯 Interview One-Liners

### What is Axios?

> Axios is a JavaScript library used to make HTTP requests from applications such as React.

### Axios vs Fetch?

> Fetch is a built-in browser API, while Axios is an external HTTP client library with features such as convenient request configuration and interceptors.

### What is `axios.create()`?

> `axios.create()` creates a reusable Axios instance with common configuration such as a base URL and headers.

### What is an interceptor?

> An interceptor allows us to execute logic before a request is sent or before a response is handled.

### How do you get response data in Axios?

```jsx
response.data
```

### How do you handle Axios errors?

> Use `try-catch` with `async/await` or Promise error handling.

---

# 🧠 Quick Revision

```text
Axios
→ HTTP client library

Install:
npm install axios

GET:
axios.get(url)

POST:
axios.post(url, data)

PUT:
axios.put(url, data)

PATCH:
axios.patch(url, data)

DELETE:
axios.delete(url)

Response:
response.data

Error:
try/catch

Reusable instance:
axios.create()

Request Interceptor:
Before request

Response Interceptor:
Before response reaches application

Environment variable:
import.meta.env.VITE_API_URL
```

# ⭐ Key Point

> **Axios is a convenient HTTP client commonly used in React applications to communicate with backend APIs.**
