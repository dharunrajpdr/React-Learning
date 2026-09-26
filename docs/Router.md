# 📘 React – React Router

## 1. What is React Router?

**React Router** is a library used to handle navigation between different pages/views in a React application.

Example:

```text
/          → Home
/about     → About
/login     → Login
/contact   → Contact
```

It allows us to build **Single Page Applications (SPA)** where the browser URL changes without completely reloading the page.

---

# 2. Why Do We Need React Router?

Without routing, we would have to manually decide which component to display.

React Router provides:

- URL-based navigation
- Multiple routes
- Dynamic routes
- Nested routes
- Navigation between components
- Protected routes
- 404 pages

---

# 3. Installing React Router

For a Vite React project:

```bash
npm install react-router-dom
```

---

# 4. Basic Routing

Import:

```jsx
import {
    BrowserRouter,
    Routes,
    Route
} from "react-router-dom";
```

Example:

```jsx
function Home() {
    return <h1>Home Page</h1>;
}

function About() {
    return <h1>About Page</h1>;
}

function App() {

    return (
        <BrowserRouter>

            <Routes>

                <Route
                    path="/"
                    element={<Home />}
                />

                <Route
                    path="/about"
                    element={<About />}
                />

            </Routes>

        </BrowserRouter>
    );
}

export default App;
```

---

# 5. Understanding the Main Components

React Router commonly uses:

```text
BrowserRouter
Routes
Route
Link
useNavigate
useParams
```

---

# 6. BrowserRouter

`BrowserRouter` enables routing in the application.

```jsx
<BrowserRouter>
    <App />
</BrowserRouter>
```

It keeps the UI synchronized with the browser URL.

---

# 7. Routes

`Routes` contains the application's route definitions.

```jsx
<Routes>

    <Route path="/" element={<Home />} />

    <Route path="/about" element={<About />} />

</Routes>
```

---

# 8. Route

`Route` connects a URL path with a component.

```jsx
<Route
    path="/about"
    element={<About />}
/>
```

Meaning:

```text
URL:
 /about

Component:
 About
```

---

# 9. Creating Multiple Pages

```jsx
function Home() {
    return <h1>Home</h1>;
}

function About() {
    return <h1>About</h1>;
}

function Contact() {
    return <h1>Contact</h1>;
}
```

Routes:

```jsx
<Routes>

    <Route path="/" element={<Home />} />

    <Route path="/about" element={<About />} />

    <Route path="/contact" element={<Contact />} />

</Routes>
```

Now:

```text
http://localhost:5173/
http://localhost:5173/about
http://localhost:5173/contact
```

display different components.

---

# 10. Link

Use `Link` for navigation inside a React application.

```jsx
import { Link } from "react-router-dom";
```

Example:

```jsx
<Link to="/">Home</Link>

<Link to="/about">About</Link>

<Link to="/contact">Contact</Link>
```

---

# 11. Why Use `Link` Instead of `<a>`?

### Normal HTML

```html
<a href="/about">About</a>
```

This can cause a full page navigation.

### React Router

```jsx
<Link to="/about">
    About
</Link>
```

This allows client-side navigation without a normal full-page reload.

### Interview Point

> `Link` is preferred for internal navigation in React Router applications.

---

# 12. Navigation Example

```jsx
function Navbar() {

    return (
        <nav>

            <Link to="/">Home</Link>

            <Link to="/about">About</Link>

            <Link to="/contact">Contact</Link>

        </nav>
    );
}
```

Then:

```jsx
function App() {

    return (
        <BrowserRouter>

            <Navbar />

            <Routes>

                <Route
                    path="/"
                    element={<Home />}
                />

                <Route
                    path="/about"
                    element={<About />}
                />

                <Route
                    path="/contact"
                    element={<Contact />}
                />

            </Routes>

        </BrowserRouter>
    );
}
```

---

# 13. `NavLink`

`NavLink` is similar to `Link`, but it can apply styling to the active route.

```jsx
import { NavLink } from "react-router-dom";
```

Example:

```jsx
<NavLink to="/">
    Home
</NavLink>

<NavLink to="/about">
    About
</NavLink>
```

It is useful for navigation bars.

---

# 14. Dynamic Routes

Sometimes the URL contains a dynamic value.

Example:

```text
/users/101
/users/102
/users/103
```

Create a dynamic route:

```jsx
<Route
    path="/users/:id"
    element={<User />}
/>
```

Here:

```text
:id
```

is a dynamic parameter.

---

# 15. `useParams`

Use `useParams()` to read dynamic route parameters.

```jsx
import { useParams } from "react-router-dom";

function User() {

    const { id } = useParams();

    return <h1>User ID: {id}</h1>;
}
```

For:

```text
/users/101
```

Output:

```text
User ID: 101
```

---

# 16. Multiple Parameters

Route:

```jsx
<Route
    path="/users/:userId/posts/:postId"
    element={<Post />}
/>
```

URL:

```text
/users/10/posts/50
```

Component:

```jsx
const { userId, postId } = useParams();
```

Values:

```text
userId = 10
postId = 50
```

---

# 17. `useNavigate`

`useNavigate()` allows navigation programmatically.

```jsx
import { useNavigate } from "react-router-dom";
```

Example:

```jsx
function Login() {

    const navigate = useNavigate();

    function handleLogin() {

        // Login logic

        navigate("/dashboard");
    }

    return (
        <button onClick={handleLogin}>
            Login
        </button>
    );
}
```

After login:

```text
Login
 ↓
navigate("/dashboard")
 ↓
Dashboard
```

---

# 18. Go Back

You can move backward in browser history:

```jsx
navigate(-1);
```

Example:

```jsx
<button onClick={() => navigate(-1)}>
    Go Back
</button>
```

---

# 19. Go Forward

```jsx
navigate(1);
```

---

# 20. Navigate with State

You can pass state while navigating:

```jsx
navigate("/profile", {
    state: {
        name: "Dharun"
    }
});
```

The destination component can read the navigation state using `useLocation`.

```jsx
import { useLocation } from "react-router-dom";

function Profile() {

    const location = useLocation();

    return (
        <h1>
            {location.state?.name}
        </h1>
    );
}
```

---

# 21. Query Parameters

Example URL:

```text
/products?category=mobile
```

You can read query parameters using `useSearchParams`.

```jsx
import { useSearchParams } from "react-router-dom";

function Products() {

    const [searchParams] = useSearchParams();

    const category =
        searchParams.get("category");

    return <h1>{category}</h1>;
}
```

Output:

```text
mobile
```

---

# 22. 404 Page

We can create a route for unknown URLs.

```jsx
<Route
    path="*"
    element={<NotFound />}
/>
```

Example:

```jsx
function NotFound() {

    return <h1>404 - Page Not Found</h1>;
}
```

If the user visits:

```text
/random-page
```

the `NotFound` component can be displayed.

---

# 23. Nested Routes

Routes can be placed inside other routes.

Example:

```text
/dashboard
/dashboard/profile
/dashboard/settings
```

Example:

```jsx
<Route
    path="/dashboard"
    element={<Dashboard />}

>
    <Route
        path="profile"
        element={<Profile />}
    />

    <Route
        path="settings"
        element={<Settings />}
    />
</Route>
```

Notice:

```jsx
path="profile"
```

instead of:

```jsx
path="/dashboard/profile"
```

because it is nested under `/dashboard`.

---

# 24. Outlet

For nested routes, `Outlet` displays the child route.

```jsx
import { Outlet } from "react-router-dom";

function Dashboard() {

    return (
        <div>

            <h1>Dashboard</h1>

            <Outlet />

        </div>
    );
}
```

Flow:

```text
/dashboard
     ↓
Dashboard

/dashboard/profile
     ↓
Dashboard
     +
Profile inside <Outlet />
```

---

# 25. React Router Flow

Remember:

```text
Browser URL
     ↓
Route matching
     ↓
Component
     ↓
UI
```

Example:

```text
/about
   ↓
<Route path="/about">
   ↓
<About />
   ↓
About Page
```

---

# 26. Common React Router Hooks

| Hook | Purpose |
|---|---|
| `useNavigate()` | Navigate programmatically |
| `useParams()` | Read URL parameters |
| `useLocation()` | Get current location/navigation state |
| `useSearchParams()` | Read/update query parameters |

---

# 27. Link vs NavLink

| Link | NavLink |
|---|---|
| Navigation | Navigation |
| Simple | Supports active styling |
| Good for normal links | Good for navbar/menu |

Example:

```jsx
<Link to="/about">
    About
</Link>
```

```jsx
<NavLink to="/about">
    About
</NavLink>
```

---

# 28. Real-World Example

A MERN application might have:

```text
/
    Home

/login
    Login

/register
    Register

/products
    Products

/products/:id
    Product Details

/cart
    Cart

/profile
    Profile

/dashboard
    Dashboard
```

React Router connects these URLs to the appropriate components.

---

# 29. Common Mistakes

### ❌ Wrong

```jsx
<Route path="/about">
    <About />
</Route>
```

Modern React Router commonly uses:

```jsx
<Route
    path="/about"
    element={<About />}
/>
```

### ❌ Wrong

```jsx
<Link href="/about">
    About
</Link>
```

### ✅ Correct

```jsx
<Link to="/about">
    About
</Link>
```

---

# 30. Interview Questions

### What is React Router?

> React Router is a library used to implement client-side routing in React applications.

### What is `BrowserRouter`?

> `BrowserRouter` provides routing functionality using the browser's history and URL.

### What is `Route`?

> `Route` maps a URL path to a React component.

### What is `Link`?

> `Link` is used for navigation between routes without normal full-page navigation.

### What is `useParams()`?

> `useParams()` is used to access dynamic parameters from the URL.

### What is `useNavigate()`?

> `useNavigate()` is used for programmatic navigation.

### What is `Outlet`?

> `Outlet` renders the matching child route inside a parent route.

---

# 🧠 Quick Revision

```text
React Router
→ Handles navigation

BrowserRouter
→ Enables routing

Routes
→ Contains routes

Route
→ URL → Component

Link
→ Navigate between routes

NavLink
→ Link + active styling

useParams
→ Read dynamic URL values

useNavigate
→ Navigate using JavaScript

useLocation
→ Read current location/state

useSearchParams
→ Handle query parameters

Outlet
→ Render nested routes

*
→ Catch-all / 404 route
```

# ⭐ Key Point

> **React Router allows a React application to have multiple URL-based views while keeping navigation client-side.**
