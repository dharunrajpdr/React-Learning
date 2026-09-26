# 📘 React – Authentication

## 1. What is Authentication?

**Authentication** means verifying who the user is.

Example:

```text
User enters email + password
          ↓
Backend verifies credentials
          ↓
Credentials correct?
       /       \
     Yes        No
      ↓          ↓
   Login      Error
```

### Simple Definition

> Authentication is the process of verifying a user's identity.

---

# 2. Authentication vs Authorization

These are different.

### Authentication

**Who are you?**

Example:

```text
Login with email and password
```

### Authorization

**What are you allowed to do?**

Example:

```text
Admin → Delete users
User  → View profile
```

### Easy Memory

```text
Authentication → Who are you?
Authorization  → What can you do?
```

---

# 3. Basic React Authentication Flow

In a MERN application:

```text
React Frontend
      ↓
Login Form
      ↓
API Request
      ↓
Node + Express Backend
      ↓
Check User
      ↓
Database
      ↓
Authentication Result
      ↓
React
```

---

# 4. Login Form

A simple login form:

```jsx
import { useState } from "react";

function Login() {
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");

  const handleSubmit = (e) => {
    e.preventDefault();

    console.log({
      email,
      password
    });
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="email"
        placeholder="Email"
        value={email}
        onChange={(e) => setEmail(e.target.value)}
      />

      <input
        type="password"
        placeholder="Password"
        value={password}
        onChange={(e) => setPassword(e.target.value)}
      />

      <button type="submit">
        Login
      </button>
    </form>
  );
}

export default Login;
```

---

# 5. Sending Login Request

Using Axios:

```jsx
import axios from "axios";

const handleSubmit = async (e) => {
  e.preventDefault();

  try {
    const response = await axios.post(
      "/api/login",
      {
        email,
        password
      }
    );

    console.log(response.data);
  } catch (error) {
    console.log(error);
  }
};
```

Flow:

```text
Form
 ↓
email + password
 ↓
POST /api/login
 ↓
Backend
```

---

# 6. Backend Authentication

The backend normally:

```text
Receive credentials
       ↓
Find user
       ↓
Verify password
       ↓
Create authentication session/token
       ↓
Send response
```

The exact implementation depends on the backend architecture.

---

# 7. Password Hashing

Passwords should **not** be stored as plain text.

❌ Bad:

```text
email: user@gmail.com
password: 123456
```

Instead, the backend stores a **password hash**.

```text
Password
   ↓
Hashing algorithm
   ↓
Password hash
   ↓
Database
```

During login:

```text
Entered password
       ↓
Password verification
       ↓
Stored hash
       ↓
Match?
```

Password hashing is normally handled by the backend using an appropriate password-hashing library.

---

# 8. Authentication with JWT

A common authentication approach is **JWT (JSON Web Token)**.

Basic flow:

```text
Login
  ↓
Backend verifies credentials
  ↓
Backend creates JWT
  ↓
Client receives authentication result
  ↓
Client uses authentication credentials
  ↓
Backend verifies them on protected requests
```

JWT contains information called **claims**.

A JWT commonly has:

```text
Header
Payload
Signature
```

---

# 9. JWT Structure

A JWT looks conceptually like:

```text
xxxxx.yyyyy.zzzzz
```

It contains three parts:

```text
Header.Payload.Signature
```

### Header

Contains information about the token.

### Payload

Contains claims.

Example:

```json
{
  "userId": "123",
  "role": "user"
}
```

### Signature

Used to verify that the token was created by a trusted issuer and has not been altered.

---

# 10. Where Can Authentication Information Be Stored?

Common approaches include:

```text
HttpOnly Cookies
Memory
Web Storage
```

The appropriate choice depends on the application's security design.

For browser-based authentication, **HttpOnly cookies** are commonly used when the backend manages cookie-based sessions or tokens.

---

# 11. HttpOnly Cookie

An `HttpOnly` cookie cannot be read by normal JavaScript running in the browser.

Example concept:

```text
Backend
   ↓
Set-Cookie
   ↓
Browser stores cookie
   ↓
Browser sends cookie with requests
```

This can reduce exposure of the cookie to JavaScript.

However, cookie-based authentication still requires proper protection against attacks such as CSRF and appropriate cookie settings.

---

# 12. Authentication State in React

React often needs to know:

```text
Is user logged in?
Who is the user?
Is authentication being checked?
```

Example:

```jsx
const [user, setUser] = useState(null);
const [loading, setLoading] = useState(true);
```

Possible states:

```text
loading = true
      ↓
Checking authentication
      ↓
User found?
   /       \
 Yes       No
 ↓          ↓
user     user = null
```

---

# 13. Auth Context

Authentication state is often shared using **Context API**.

Example:

```jsx
import { createContext, useContext, useState } from "react";

const AuthContext = createContext();

export function AuthProvider({ children }) {
  const [user, setUser] = useState(null);

  return (
    <AuthContext.Provider value={{ user, setUser }}>
      {children}
    </AuthContext.Provider>
  );
}

export function useAuth() {
  return useContext(AuthContext);
}
```

Now components can use:

```jsx
const { user } = useAuth();
```

---

# 14. Authentication Provider Structure

Usually:

```jsx
function App() {
  return (
    <AuthProvider>
      <Router />
    </AuthProvider>
  );
}
```

Flow:

```text
AuthProvider
      ↓
Authentication State
      ↓
All child components
```

---

# 15. Checking Current User

After the application starts, React may call an API such as:

```text
GET /api/me
```

Example:

```jsx
useEffect(() => {
  const getCurrentUser = async () => {
    try {
      const response = await axios.get("/api/me");

      setUser(response.data.user);
    } catch (error) {
      setUser(null);
    } finally {
      setLoading(false);
    }
  };

  getCurrentUser();
}, []);
```

This allows the application to restore the authentication state after a page refresh.

---

# 16. Login State

After successful login:

```jsx
const login = async (email, password) => {
  const response = await axios.post("/api/login", {
    email,
    password
  });

  setUser(response.data.user);
};
```

Then:

```text
Login successful
      ↓
setUser()
      ↓
AuthContext updates
      ↓
Components receive new user
```

---

# 17. Logout

Logout usually involves:

```text
User clicks Logout
       ↓
Frontend sends logout request
       ↓
Backend invalidates session / clears cookie
       ↓
Frontend clears user state
       ↓
User becomes unauthenticated
```

Example:

```jsx
const logout = async () => {
  try {
    await axios.post("/api/logout");
    setUser(null);
  } catch (error) {
    console.log(error);
  }
};
```

---

# 18. Conditional Rendering Based on Authentication

Example:

```jsx
function Navbar() {
  const { user } = useAuth();

  return (
    <nav>
      {user ? (
        <button>Logout</button>
      ) : (
        <button>Login</button>
      )}
    </nav>
  );
}
```

Flow:

```text
user exists?
   ↓
Yes → Show Logout
No  → Show Login
```

---

# 19. Authentication Loading State

Important:

Don't immediately assume the user is logged out while the application is still checking authentication.

Example:

```jsx
if (loading) {
  return <p>Checking authentication...</p>;
}

return user ? <Dashboard /> : <Login />;
```

Flow:

```text
App starts
   ↓
Check authentication
   ↓
Loading
   ↓
Authentication result
   ↓
Show correct UI
```

---

# 20. Authentication and Protected Routes

Some pages should only be available to authenticated users.

Examples:

```text
/dashboard
/profile
/orders
/settings
```

A protected route can check:

```jsx
if (!user) {
  return <Navigate to="/login" />;
}
```

Conceptually:

```text
User requests /dashboard
          ↓
Is user authenticated?
       /       \
     Yes        No
      ↓          ↓
Dashboard     Login
```

Protected routes will be covered in detail in the next topic.

---

# 21. Authentication vs Protected Route

### Authentication

Determines:

```text
Is the user logged in?
```

### Protected Route

Determines:

```text
Can the user access this page?
```

They work together.

```text
Authentication
      ↓
User information
      ↓
Protected Route
      ↓
Allow / Redirect
```

---

# 22. Common Authentication Components

A React application may contain:

```text
Login
Register
Logout
Forgot Password
Reset Password
Profile
AuthProvider
ProtectedRoute
```

---

# 23. Common Authentication API Endpoints

A typical backend may have:

```text
POST /api/register
POST /api/login
POST /api/logout
GET  /api/me
POST /api/forgot-password
POST /api/reset-password
```

The exact routes depend on the application.

---

# 24. Authentication Security Basics

Important practices:

```text
Never store plain-text passwords
Use HTTPS
Validate input
Use secure authentication mechanisms
Use HttpOnly cookies when appropriate
Configure cookie security attributes properly
Protect against CSRF when using cookies
Handle authentication errors safely
Use short-lived credentials where appropriate
```

Also:

> Frontend authentication checks improve the user experience, but the backend must enforce authorization.

For example, hiding an Admin button in React does **not** prevent a user from manually calling an admin API.

---

# 25. Frontend vs Backend Security

### Frontend

Can:

```text
Show Login
Show Dashboard
Hide UI
Redirect users
Display user information
```

### Backend

Must enforce:

```text
Authentication
Authorization
Permissions
Protected API access
```

Important:

```text
Frontend protection ≠ Backend security
```

---

# 26. Common Mistakes

### ❌ Mistake 1: Storing plain-text passwords

Never do this.

### ❌ Mistake 2: Trusting frontend authorization

This is unsafe:

```jsx
if (user.role === "admin") {
  // assume backend is protected
}
```

The backend must independently verify permissions.

### ❌ Mistake 3: Forgetting loading state

This can cause incorrect redirects while authentication is still being checked.

### ❌ Mistake 4: Exposing sensitive credentials unnecessarily

Use secure authentication mechanisms and avoid exposing sensitive tokens or secrets to client-side JavaScript unless the architecture specifically requires it.

---

# 27. Authentication Flow – Complete

```text
              REGISTER
                 ↓
          User creates account
                 ↓
             Database
                 ↓
              LOGIN
                 ↓
        Backend verifies user
                 ↓
       Authentication established
                 ↓
          React gets user
                 ↓
          AuthContext state
                 ↓
       Protected application
                 ↓
              LOGOUT
                 ↓
     Authentication cleared
```

---

# 28. Interview Questions

### Q1. What is authentication?

> Authentication is the process of verifying the identity of a user.

### Q2. What is authorization?

> Authorization determines what an authenticated user is allowed to access or perform.

### Q3. Authentication vs authorization?

```text
Authentication → Who are you?
Authorization  → What can you do?
```

### Q4. Why use AuthContext?

To share authentication state and related functions across components without prop drilling.

### Q5. Why do we need a loading state?

To wait until the application finishes checking the current authentication state before deciding what UI or route to show.

### Q6. Can React alone secure an application?

No.

The backend must enforce authentication and authorization for protected resources.

### Q7. What is JWT?

> JWT is a token format commonly used to carry claims between parties and can be used as part of an authentication system.

### Q8. Should passwords be stored directly in the database?

No. Passwords should be securely hashed before storage.

---

# 29. Quick Revision

```text
Authentication
      ↓
Verify user identity
      ↓
Login
      ↓
Authentication established
      ↓
Store/share auth state
      ↓
Protected Routes
      ↓
Access protected pages
      ↓
Logout
      ↓
Clear authentication
```

### Easy Memory

```text
Authentication → Who are you?
Authorization  → What can you do?
AuthContext    → Share auth state
Protected Route → Control page access
```

# 🎯 Interview One-Liner

> **Authentication verifies a user's identity, while authorization determines what that authenticated user is allowed to access or perform.**

# ⭐ Key Point

```text
Frontend → UI and routing experience
Backend  → Actual security enforcement
```
