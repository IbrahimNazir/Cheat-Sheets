# **React CLI & Components Cheat Sheet**

This cheat sheet covers **React CLI commands**, **component patterns**, **hooks**, and **best practices** for building scalable React apps.

---

## **1. React CLI Commands**

### **Create React App (CRA)**

| Command                       | Description                                 |
| ----------------------------- | ------------------------------------------- |
| `npx create-react-app my-app` | Create a new React project                  |
| `npm start`                   | Start development server (`localhost:3000`) |
| `npm run build`               | Build for production (`/build` folder)      |
| `npm test`                    | Run Jest tests                              |
| `npm run eject`               | Eject from CRA (irreversible!)              |

### **Vite (Faster Alternative to CRA)**

| Command                  | Description               |
| ------------------------ | ------------------------- |
| `npm create vite@latest` | Create a new Vite project |
| `npm run dev`            | Start dev server          |
| `npm run build`          | Build for production      |

### **Next.js CLI**

| Command                      | Description          |
| ---------------------------- | -------------------- |
| `npx create-next-app@latest` | Create a Next.js app |
| `npm run dev`                | Start dev server     |
| `npm run build`              | Build for production |

---

## **2. React Component Patterns**

### **Functional Components (Recommended)**

```jsx
import { useState, useEffect } from "react";

function MyComponent({ prop1, prop2 }) {
  const [state, setState] = useState("");

  useEffect(() => {
    // Side effects (API calls, subscriptions)
    return () => {
      /* Cleanup */
    };
  }, [dependency]);

  return <div>{state}</div>;
}

export default MyComponent;
```

### **Class Components (Legacy)**

```jsx
import React from "react";

class MyComponent extends React.Component {
  state = { count: 0 };

  componentDidMount() {
    // Runs after first render
  }

  render() {
    return <div>{this.state.count}</div>;
  }
}
```

### **Component Types**

| Type                   | Use Case                 | Example                |
| ---------------------- | ------------------------ | ---------------------- |
| **Presentational**     | UI-only, no logic        | `Button`, `Card`       |
| **Container**          | Manages data/logic       | `UserProfileContainer` |
| **Higher-Order (HOC)** | Reuse logic              | `withAuth(Component)`  |
| **Compound**           | Group related components | `Tabs`, `Accordion`    |

---

## **3. React Hooks Cheat Sheet**

### **Core Hooks**

| Hook         | Purpose                   | Example                                                        |
| ------------ | ------------------------- | -------------------------------------------------------------- |
| `useState`   | Manage state              | `const [count, setCount] = useState(0);`                       |
| `useEffect`  | Side effects              | `useEffect(() => { fetchData(); }, []);`                       |
| `useContext` | Access context            | `const theme = useContext(ThemeContext);`                      |
| `useRef`     | DOM access / mutable refs | `const inputRef = useRef();`                                   |
| `useReducer` | Complex state logic       | `const [state, dispatch] = useReducer(reducer, initialState);` |

### **Custom Hooks (Reusable Logic)**

```jsx
function useFetch(url) {
  const [data, setData] = useState(null);

  useEffect(() => {
    fetch(url)
      .then((res) => res.json())
      .then(setData);
  }, [url]);

  return data;
}

// Usage:
const userData = useFetch("/api/user");
```

---

## **4. Props & State Management**

### **Passing Props**

```jsx
// Parent
<ChildComponent name="John" age={25} />;

// Child
function ChildComponent({ name, age }) {
  return (
    <p>
      {name} is {age} years old.
    </p>
  );
}
```

### **Lifting State Up**

```jsx
function Parent() {
  const [count, setCount] = useState(0);
  return <Child count={count} setCount={setCount} />;
}

function Child({ count, setCount }) {
  return <button onClick={() => setCount((c) => c + 1)}>Count: {count}</button>;
}
```

---

## **5. Styling Components**

### **CSS Modules (Recommended)**

```jsx
import styles from "./Button.module.css";

function Button() {
  return <button className={styles.primary}>Click</button>;
}
```

### **Styled Components (CSS-in-JS)**

```jsx
import styled from "styled-components";

const StyledButton = styled.button`
  background: ${(props) => (props.primary ? "blue" : "gray")};
`;

<StyledButton primary>Click</StyledButton>;
```

### **Tailwind CSS (Utility-First)**

```jsx
<button className="bg-blue-500 text-white p-2 rounded">Click</button>
```

---

## **6. Performance Optimization**

| Technique        | Code Example                                              |
| ---------------- | --------------------------------------------------------- |
| **React.memo**   | `const MemoizedComp = React.memo(Component)`              |
| **useCallback**  | `const memoizedFn = useCallback(() => {}, [deps])`        |
| **useMemo**      | `const memoizedVal = useMemo(() => compute(), [deps])`    |
| **Lazy Loading** | `const LazyComp = React.lazy(() => import("./LazyComp"))` |

---

## **7. Folder Structure (Best Practices)**

```markdown
src/  
├── components/  
│ ├── ui/ # Reusable UI (Button, Card)  
│ └── features/ # Feature-specific (UserProfile)  
├── hooks/ # Custom hooks (useFetch, useAuth)  
├── pages/ # Route-based components  
├── utils/ # Helpers (formatters, validators)  
├── services/ # API calls (axios, fetch)  
├── contexts/ # React Context providers  
├── assets/ # Images, fonts  
└── App.jsx # Main app entry
```

---

## **8. React Router (v6)**

```jsx
import { BrowserRouter, Routes, Route } from "react-router-dom";

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
      </Routes>
    </BrowserRouter>
  );
}
```
