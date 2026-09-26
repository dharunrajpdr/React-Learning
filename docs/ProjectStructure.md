# 📘 React – Project Structure

## 1. What is React Project Structure?

A React project contains different **folders and files**, and each one has a specific purpose.

A basic Vite + React project looks like:

```text
my-react-app/
│
├── node_modules/
├── public/
├── src/
│   ├── assets/
│   ├── App.jsx
│   ├── main.jsx
│   └── index.css
│
├── .gitignore
├── index.html
├── package.json
├── package-lock.json
└── vite.config.js
```

---

# 2. `node_modules`

```text
node_modules/
```

This folder contains the **installed packages and dependencies** used by the project.

It is created when we run:

```bash
npm install
```

### Important

We normally don't manually modify this folder.

It is also usually added to `.gitignore`.

---

# 3. `public/`

```text
public/
```

This folder contains static files that can be served directly.

Examples:

```text
Images
Icons
Favicon
Other static files
```

Example:

```text
public/
├── logo.png
└── favicon.ico
```

---

# 4. `src/`

```text
src/
```

This is the **main development folder** of a React application.

Most of our React code will be written inside `src`.

Example:

```text
src/
├── assets/
├── components/
├── pages/
├── App.jsx
├── main.jsx
└── index.css
```

---

# 5. `src/assets/`

```text
src/assets/
```

Used for assets that are imported into React components.

Examples:

```text
Images
SVG files
Fonts
Other frontend assets
```

Example:

```jsx
import logo from "./assets/logo.png";
```

---

# 6. `App.jsx`

```text
src/App.jsx
```

`App.jsx` is commonly used as the **main/root React component** of the application.

Example:

```jsx
function App() {
    return (
        <h1>Hello React</h1>
    );
}

export default App;
```

---

# 7. `main.jsx`

```text
src/main.jsx
```

`main.jsx` is the **entry point** that starts the React application and renders the root component.

Example:

```jsx
import { StrictMode } from 'react';
import { createRoot } from 'react-dom/client';
import App from './App.jsx';

createRoot(document.getElementById('root')).render(
    <StrictMode>
        <App />
    </StrictMode>
);
```

### Flow

```text
main.jsx
   ↓
App.jsx
   ↓
React Components
   ↓
UI
```

---

# 8. `index.css`

```text
src/index.css
```

This file can contain **global CSS styles**.

Example:

```css
body {
    margin: 0;
    font-family: Arial, sans-serif;
}
```

---

# 9. `index.html`

```text
index.html
```

This is the main HTML page used by the Vite application.

It contains the root element:

```html
<div id="root"></div>
```

React renders the application inside this element.

```text
index.html
     ↓
<div id="root">
     ↓
React Application
```

---

# 10. `package.json`

```text
package.json
```

Contains project information such as:

- Project name
- Version
- Scripts
- Dependencies
- Development dependencies

Example:

```json
{
    "scripts": {
        "dev": "vite",
        "build": "vite",
        "preview": "vite"
    }
}
```

---

# 11. `package-lock.json`

```text
package-lock.json
```

It records the **exact dependency versions** installed for the project.

It helps maintain consistent installations across different environments.

---

# 12. `vite.config.js`

```text
vite.config.js
```

This file contains configuration for Vite.

Example:

```javascript
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';

export default defineConfig({
    plugins: [react()]
});
```

---

# 13. `.gitignore`

```text
.gitignore
```

This file tells Git which files or folders should not be tracked.

Example:

```text
node_modules/
dist/
.env
```

---

# 14. Common React Folder Structure

As the project becomes larger, we can organize `src` like this:

```text
src/
│
├── assets/
│
├── components/
│   ├── Navbar.jsx
│   ├── Button.jsx
│   └── Card.jsx
│
├── pages/
│   ├── Home.jsx
│   ├── Login.jsx
│   └── Profile.jsx
│
├── hooks/
│
├── services/
│
├── utils/
│
├── App.jsx
├── main.jsx
└── index.css
```

### Purpose

| Folder | Purpose |
|---|---|
| `assets` | Images and other assets |
| `components` | Reusable components |
| `pages` | Application pages |
| `hooks` | Custom React hooks |
| `services` | API/service logic |
| `utils` | Helper functions |
| `App.jsx` | Main application component |
| `main.jsx` | Application entry point |

---

# 15. How React Application Starts

The basic flow is:

```text
index.html
     ↓
<div id="root">
     ↓
main.jsx
     ↓
<App />
     ↓
Other Components
     ↓
Browser UI
```

---

# 🧠 Easy Memory Trick

```text
index.html → HTML entry point

main.jsx   → Starts React

App.jsx    → Main component

src/       → React source code

public/    → Static files

assets/    → Images/assets

components/ → Reusable UI

pages/     → Application pages

package.json → Project & dependencies
```

---

# 🎯 Interview One-Liners

### What is `main.jsx`?

> `main.jsx` is the entry point that renders the React application into the root DOM element.

### What is `App.jsx`?

> `App.jsx` commonly contains the root component of the React application.

### What is the `src` folder?

> `src` contains the main source code of the React application.

### What is `public`?

> `public` contains static files that can be served directly.

### What is `package.json`?

> `package.json` contains project metadata, scripts, and dependency information.

### What is `node_modules`?

> `node_modules` contains the installed project dependencies.

---

# 🔥 Quick Revision

```text
index.html
    ↓
main.jsx
    ↓
App.jsx
    ↓
Components
    ↓
UI
```

```text
src/        → Main source code
components/ → Reusable UI
pages/      → Pages
assets/     → Assets
public/     → Static files
```

# ⭐ Key Point

> **`main.jsx` starts the React application, `App.jsx` represents the main component, and most React development happens inside `src/`.**
