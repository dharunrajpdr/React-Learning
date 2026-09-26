# 📘 React – Loading & Error Handling

## 1. Why Do We Need Loading & Error Handling?

When React calls an API, the request takes some time.

During this time:

```text
API Request
    ↓
Loading
    ↓
Success OR Error
```

We should show the user what is happening.

Example:

```text
Loading...
```

If something fails:

```text
Failed to load users
```

---

# 2. Three Important States

For API calls, we commonly manage:

```jsx
const [data, setData] = useState([]);
const [loading, setLoading] = useState(true);
const [error, setError] = useState("");
```

They represent:

```text
data
→ API response

loading
→ Request in progress

error
→ Request failed
```

---

# 3. Basic Loading Example

```jsx
import { useState } from "react";

function App() {

    const [loading, setLoading] = useState(false);

    async function handleClick() {

        setLoading(true);

        // API call

        setLoading(false);
    }

    return (
        <div>

            {loading && <p>Loading...</p>}

            <button onClick={handleClick}>
                Get Data
            </button>

        </div>
    );
}
```

---

# 4. Loading with Conditional Rendering

We can use:

```jsx
if (loading) {
    return <p>Loading...</p>;
}
```

Example:

```jsx
function Users() {

    if (loading) {
        return <h2>Loading...</h2>;
    }

    return <h2>Users</h2>;
}
```

---

# 5. Basic Error Handling

Use an error state:

```jsx
const [error, setError] = useState("");
```

If something fails:

```jsx
setError("Something went wrong");
```

Then:

```jsx
if (error) {
    return <p>{error}</p>;
}
```

---

# 6. Complete API Example

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

        async function fetchUsers() {

            try {

                const response =
                    await axios.get(
                        "https://example.com/api/users"
                    );

                setUsers(response.data);

            } catch (error) {

                setError(
                    "Failed to load users"
                );

            } finally {

                setLoading(false);

            }
        }

        fetchUsers();

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

# 7. Understanding `finally`

`finally` executes whether the request succeeds or fails.

```jsx
try {

    // API request

} catch (error) {

    // Handle error

} finally {

    setLoading(false);

}
```

This is useful because:

```text
Success → stop loading
Error   → stop loading
```

---

# 8. API Flow

```text
Component Loads
      ↓
loading = true
      ↓
API Request
      ↓
 ┌───────────────┐
 │               │
Success        Error
 │               │
 ↓               ↓
setData()     setError()
 │               │
 └───────┬───────┘
         ↓
loading = false
         ↓
Display UI
```

---

# 9. Error Message from Axios

Axios errors can provide useful information.

```jsx
catch (error) {

    setError(
        error.response?.data?.message ||
        "Something went wrong"
    );

}
```

The `?.` is optional chaining.

It prevents an error if some property does not exist.

---

# 10. HTTP Status Codes

Some common API status codes:

| Status | Meaning |
|---|---|
| 200 | OK |
| 201 | Created |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 500 | Server Error |

Example:

```jsx
catch (error) {

    if (error.response?.status === 401) {
        setError("Please login first");
    }

}
```

---

# 11. Different Errors

We can display different messages.

```jsx
catch (error) {

    if (error.response?.status === 404) {

        setError("Data not found");

    }
    else if (error.response?.status === 401) {

        setError("Please login");

    }
    else {

        setError("Something went wrong");

    }
}
```

---

# 12. Empty Data Handling

Sometimes the API succeeds but returns no data.

Example:

```jsx
if (users.length === 0) {
    return <p>No users found.</p>;
}
```

Complete flow:

```text
Loading
   ↓
Error?
   ↓
No Data?
   ↓
Display Data
```

---

# 13. Better UI Flow

A common structure is:

```jsx
if (loading) {
    return <p>Loading...</p>;
}

if (error) {
    return <p>{error}</p>;
}

if (users.length === 0) {
    return <p>No users found.</p>;
}

return (
    <div>
        {/* Display users */}
    </div>
);
```

This makes the UI easy to understand.

---

# 14. Retry Button

Sometimes the user should be able to retry the failed request.

Example:

```jsx
function ErrorMessage({ onRetry }) {

    return (
        <div>

            <p>Failed to load data.</p>

            <button onClick={onRetry}>
                Retry
            </button>

        </div>
    );
}
```

Then:

```jsx
<ErrorMessage
    onRetry={fetchUsers}
/>
```

---

# 15. Loading Button

For POST/login operations, the button itself can show loading.

```jsx
const [loading, setLoading] =
    useState(false);
```

```jsx
<button
    onClick={handleLogin}
    disabled={loading}
>
    {loading ? "Logging in..." : "Login"}
</button>
```

Output:

```text
Before request:

[ Login ]

During request:

[ Logging in... ]
```

---

# 16. Prevent Multiple Requests

Disable the button while loading:

```jsx
<button
    disabled={loading}
    onClick={handleSubmit}
>
    {loading ? "Submitting..." : "Submit"}
</button>
```

This can help prevent accidental repeated submissions.

---

# 17. Loading Spinner

Instead of text:

```jsx
{loading && <Spinner />}
```

Example:

```jsx
function Spinner() {

    return (
        <div>
            Loading...
        </div>
    );
}
```

Then:

```jsx
if (loading) {
    return <Spinner />;
}
```

---

# 18. Separate Components

For larger applications, we can create reusable components:

```text
components/
├── Loader.jsx
├── ErrorMessage.jsx
└── EmptyState.jsx
```

Example:

```jsx
<Loader />

<ErrorMessage />

<EmptyState />
```

This improves code organization.

---

# 19. Loading vs Error

| Loading | Error |
|---|---|
| Request is in progress | Request failed |
| Show spinner/message | Show error message |
| Usually disable action | Provide retry if useful |

---

# 20. Common Mistakes

### ❌ Forgetting to stop loading

```jsx
try {
    const response = await axios.get(url);
    setData(response.data);
}
```

If we never update loading:

```text
Loading...
```

may remain forever.

### ✅ Better

```jsx
try {

    const response =
        await axios.get(url);

    setData(response.data);

} catch (error) {

    setError("Failed");

} finally {

    setLoading(false);

}
```

---

# 21. Don't Ignore Errors

### ❌

```jsx
catch (error) {
    console.log(error);
}
```

For a real UI, it is usually better to provide user feedback:

```jsx
catch (error) {
    setError("Unable to load data");
}
```

---

# 22. Real MERN Example

Suppose:

```text
React
 ↓
Axios
 ↓
Express
 ↓
MongoDB
```

If MongoDB/server has a problem:

```text
API Request
    ↓
Server Error
    ↓
Axios catches error
    ↓
setError()
    ↓
React displays message
```

Example:

```text
Unable to load products.
Please try again.
```

---

# 🎯 Interview One-Liners

### Why do we need loading state?

> Loading state tells the user that an asynchronous operation is currently in progress.

### Why do we need error state?

> Error state allows the application to display useful feedback when an operation fails.

### Why use `finally`?

> `finally` runs after either success or failure, so it is useful for stopping the loading state.

### What is a retry mechanism?

> A retry mechanism allows the user to attempt a failed operation again.

### What is an empty state?

> An empty state is UI shown when a request succeeds but there is no data to display.

---

# 🧠 Quick Revision

```text
API Request
    ↓
Loading
    ↓
Success / Error
```

Common states:

```jsx
data
loading
error
```

Common pattern:

```jsx
try {
    // API request
}
catch (error) {
    // Handle error
}
finally {
    // Stop loading
}
```

UI states:

```text
Loading
Error
Empty
Success
```

# ⭐ Key Point

> **Good React applications should handle all important API states: loading, error, empty data, and successful data.**
