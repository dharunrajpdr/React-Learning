# 📘 React – Environment Variables

## 🔹 33. Environment Variables

### 1. What are Environment Variables?

**Environment variables** are values stored outside the main source code that are used for configuration.

They are useful when a value changes between environments like:

```text
Development → Local API
Production  → Live API
```

Instead of hardcoding the value in React, we can store it in an environment file.

---

## 🔹 2. Why do we use Environment Variables?

Environment variables help us:

- Store API URLs
- Manage different configurations
- Avoid hardcoding configuration values
- Use different values for development and production
- Keep configuration separate from application code

### Without Environment Variable

```js
const API_URL = "http://localhost:5000/api";
```

### With Environment Variable

```js
const API_URL = import.meta.env.VITE_API_URL;
```

---

## 🔹 3. Environment Variables in Vite

For a Vite React project, frontend environment variables should start with:

```text
VITE_
```

Example:

```env
VITE_API_URL=http://localhost:5000/api
```

---

## 🔹 4. Creating a `.env` File

Create `.env` in the project root:

```text
my-react-app/
├── src/
├── public/
├── .env
├── package.json
└── vite.config.js
```

Example `.env`:

```env
VITE_API_URL=http://localhost:5000/api
VITE_APP_NAME=My React App
```

---

## 🔹 5. Accessing Environment Variables

Use:

```js
import.meta.env
```

Example:

```js
const apiUrl = import.meta.env.VITE_API_URL;

console.log(apiUrl);
```

Output:

```text
http://localhost:5000/api
```

Another example:

```js
const appName = import.meta.env.VITE_APP_NAME;

console.log(appName);
```

Output:

```text
My React App
```

---

## 🔹 6. Using Environment Variables with Axios

### `.env`

```env
VITE_API_URL=http://localhost:5000/api
```

### Axios

```js
import axios from "axios";

const api = axios.create({
    baseURL: import.meta.env.VITE_API_URL
});

export default api;
```

Now:

```js
api.get("/users");
```

will call:

```text
http://localhost:5000/api/users
```

---

## 🔹 7. Different Environment Files

Common environment files include:

```text
.env
.env.local
.env.development
.env.production
```

### `.env.development`

```env
VITE_API_URL=http://localhost:5000/api
```

### `.env.production`

```env
VITE_API_URL=https://example.com/api
```

This allows us to use different configuration values for different environments.

---

## 🔹 8. Important `VITE_` Rule

This variable:

```env
VITE_API_URL=http://localhost:5000/api
```

can be accessed in React:

```js
import.meta.env.VITE_API_URL
```

But:

```env
API_URL=http://localhost:5000/api
```

is not exposed to frontend code through Vite's client-side environment API.

### Easy Memory

```text
VITE_ → Available to frontend
```

---

## 🔹 9. Can We Store Passwords in React `.env`?

### ❌ No

Do not store sensitive secrets in frontend environment variables.

For example:

```env
VITE_DB_PASSWORD=123456
VITE_JWT_SECRET=mysecret
VITE_PRIVATE_API_KEY=secret
```

These should **not** be treated as secret.

Why?

Because frontend environment variables are included in the client-side application during the build.

Users can potentially inspect the built application.

---

## 🔹 10. What Can We Store?

Examples of frontend configuration:

```env
VITE_API_URL=https://api.example.com
VITE_APP_NAME=Chatify
VITE_APP_VERSION=1.0.0
```

These are configuration values, not secret credentials.

### Backend Secrets

Sensitive values should be stored on the backend:

```env
DB_PASSWORD=******
JWT_SECRET=******
PRIVATE_API_KEY=******
```

---

## 🔹 11. `.env` and `.gitignore`

If `.env` contains local or private configuration, it is commonly added to `.gitignore`.

```gitignore
.env
.env.local
```

Example:

```text
node_modules/
dist/
.env
.env.local
```

This prevents Git from tracking those files.

---

## 🔹 12. Environment Variables in React Component

`.env`:

```env
VITE_APP_NAME=Chatify
```

React:

```jsx
function App() {

    const appName = import.meta.env.VITE_APP_NAME;

    return (
        <h1>{appName}</h1>
    );
}

export default App;
```

Output:

```text
Chatify
```

---

## 🔹 13. Environment Variables in a MERN Application

Typical flow:

```text
React Frontend
      ↓
Environment Variable
      ↓
API URL
      ↓
Node + Express Backend
      ↓
MongoDB
```

Example:

```env
VITE_API_URL=http://localhost:5000/api
```

React:

```js
axios.get(`${import.meta.env.VITE_API_URL}/users`);
```

Backend:

```text
http://localhost:5000/api/users
```

---

## 🔹 14. Restart Development Server

After changing environment variables, restart the development server if necessary.

```bash
npm run dev
```

Stop the server:

```text
Ctrl + C
```

Then start again:

```bash
npm run dev
```

---

## 🔹 15. Common Mistakes

### ❌ Mistake 1: Missing `VITE_`

```env
API_URL=http://localhost:5000
```

Use:

```env
VITE_API_URL=http://localhost:5000
```

---

### ❌ Mistake 2: Using `process.env`

In Vite React, normally use:

```js
import.meta.env.VITE_API_URL
```

instead of:

```js
process.env.VITE_API_URL
```

---

### ❌ Mistake 3: Putting secrets in frontend variables

```env
VITE_PASSWORD=secret123
```

❌ Do not store sensitive secrets in frontend environment variables.

---

### ❌ Mistake 4: Forgetting `.gitignore`

If the environment file contains private configuration:

```gitignore
.env
.env.local
```

---

## 🔹 16. Hardcoded Value vs Environment Variable

| Hardcoded | Environment Variable |
|---|---|
| Value is directly in code | Value is stored separately |
| Difficult to change | Easy to change |
| Not convenient for multiple environments | Supports different environments |
| Configuration mixed with code | Configuration separated from code |

---

## 🔹 17. Environment Variables vs Secrets

```text
Environment Variable
        ↓
Configuration value
        ↓
Can be public in frontend
```

But:

```text
Secret
   ↓
Password / private key / JWT secret
   ↓
Must remain on backend/server
```

---

# 🎯 Interview Questions

### 1. What are environment variables?

> Environment variables store configuration values outside the application source code and allow different configurations for different environments.

### 2. How do you access environment variables in Vite React?

```js
import.meta.env.VITE_API_URL
```

### 3. Why does Vite use the `VITE_` prefix?

> Vite exposes client-side environment variables that use the `VITE_` prefix.

### 4. Can we store secrets in React environment variables?

> No. Frontend environment variables are bundled into client-side code and should not be considered secret.

### 5. Where should database credentials be stored?

> Database credentials should be stored on the backend/server environment, not in frontend code.

### 6. Why are environment variables useful?

> They allow configuration such as API URLs to change between development and production without changing the application source code.

---

# 🧠 Quick Revision

```text
Environment Variables
        ↓
Store configuration separately
        ↓
Vite React
        ↓
.env
        ↓
VITE_API_URL=...
        ↓
import.meta.env.VITE_API_URL
```

### ⭐ Remember

```text
VITE_ → Frontend-accessible variable

Frontend → Don't store secrets

Backend → Store sensitive credentials

.env → Configuration

.gitignore → Prevent private local files from being committed
```

### ⭐ Interview One-Liner

> **Environment variables allow React applications to use different configuration values for different environments without hardcoding them directly into the source code.**
