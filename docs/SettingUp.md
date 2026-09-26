# 📘 React – Setting Up a React Project

## 1. What Do We Need?

To create a React project, we mainly need:

```text
1. Node.js
2. npm
3. Code Editor
4. Browser
```

### Node.js

Node.js allows us to run JavaScript outside the browser.

It also provides **npm (Node Package Manager)**, which is used to install React and other packages.

---

# 2. Check Node.js Installation

Open the terminal and run:

```bash
node -v
```

Example:

```text
v22.x.x
```

Check npm:

```bash
npm -v
```

Example:

```text
10.x.x
```

If both commands return a version, Node.js and npm are available.

---

# 3. Create a React Project

A common modern way to create a React project is using **Vite**.

Run:

```bash
npm create vite@latest my-react-app
```

You will be asked some questions.

Example:

```text
Project name: my-react-app

Select a framework:
> React

Select a variant:
> JavaScript
```

---

# 4. Move Into the Project

```bash
cd my-react-app
```

---

# 5. Install Dependencies

Run:

```bash
npm install
```

This installs the packages required by the project.

---

# 6. Start the Development Server

Run:

```bash
npm run dev
```

You will get a local URL similar to:

```text
http://localhost:5173/
```

Open this URL in your browser.

---

# 7. Complete Setup

The complete process is:

```bash
npm create vite@latest my-react-app

cd my-react-app

npm install

npm run dev
```

### Flow

```text
Create Project
      ↓
cd Project
      ↓
npm install
      ↓
npm run dev
      ↓
Browser
      ↓
React Application
```

---

# 8. Important Commands

| Command | Purpose |
|---|---|
| `npm create vite@latest` | Create a Vite project |
| `cd project-name` | Move into project folder |
| `npm install` | Install dependencies |
| `npm run dev` | Start development server |
| `npm run build` | Create production build |
| `npm run preview` | Preview production build |

---

# 9. What is Vite?

**Vite** is a modern frontend build tool commonly used for React projects.

It provides:

- Fast development server
- Fast development experience
- Project setup
- Production build support
- Hot Module Replacement (HMR)

### Simple Definition

> Vite is a frontend build tool used to develop and build modern web applications.

---

# 10. What is npm?

**npm** stands for **Node Package Manager**.

It is used to:

- Install packages
- Manage dependencies
- Run project scripts
- Manage project packages

Example:

```bash
npm install axios
```

This installs the Axios package.

---

# 11. What is package.json?

`package.json` contains important information about the project.

Example:

```json
{
    "name": "my-react-app",
    "version": "1.0.0",
    "scripts": {
        "dev": "vite"
    }
}
```

It can contain:

```text
Project information
Dependencies
Development dependencies
Scripts
Version information
```

---

# 12. What is node_modules?

After running:

```bash
npm install
```

a folder called:

```text
node_modules/
```

is created.

It contains the packages and dependencies required by the project.

### Important

Usually, we **do not manually edit** the `node_modules` folder.

---

# 13. Basic React Project Structure

After creating a Vite React project:

```text
my-react-app/
│
├── node_modules/
│
├── public/
│
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

We will learn these files in the next topic.

---

# 14. Development vs Production

### Development

```bash
npm run dev
```

Used while developing the application.

### Production Build

```bash
npm run build
```

Creates an optimized production build.

---

# 🧠 Easy Memory Trick

```text
Node.js
   ↓
npm
   ↓
Vite
   ↓
React Project
   ↓
npm run dev
   ↓
Browser
```

---

# 🎯 Interview One-Liners

### What is Vite?

> Vite is a modern frontend build tool used to develop and build web applications.

### What is npm?

> npm is the Node Package Manager used to install and manage JavaScript packages and project dependencies.

### How do you create a React project using Vite?

```bash
npm create vite@latest my-react-app
```

### How do you start a React development server?

```bash
npm run dev
```

### What is node_modules?

> `node_modules` contains the packages and dependencies installed for the project.

### What is package.json?

> `package.json` contains project metadata, scripts, and dependency information.

---

# 🔥 Quick Revision

```text
Node.js → JavaScript runtime
npm     → Package manager
Vite    → Build tool
React   → UI library

Create:
npm create vite@latest my-react-app

Install:
npm install

Run:
npm run dev

Build:
npm run build
```

# ⭐ Key Point

> **Vite + React provides a simple and fast setup for developing modern React applications.**
