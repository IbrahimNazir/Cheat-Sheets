# **Express.js Cheat Sheet**

## **1. Installation & Setup**

```bash
# Initialize a Node.js project (if not already done)
npm init -y

# Install Express
npm install express

# Install nodemon (for auto-reload in development)
npm install --save-dev nodemon

# Basic server setup (app.js)
const express = require('express');
const app = express();
const PORT = 3000;

app.listen(PORT, () => {
  console.log(`Server running on http://localhost:${PORT}`);
});
```

---

## **2. Basic Routing**

| Method   | Example                                           | Description |
| -------- | ------------------------------------------------- | ----------- |
| `GET`    | `app.get('/users', (req, res) => { ... })`        | Fetch data  |
| `POST`   | `app.post('/users', (req, res) => { ... })`       | Create data |
| `PUT`    | `app.put('/users/:id', (req, res) => { ... })`    | Update data |
| `DELETE` | `app.delete('/users/:id', (req, res) => { ... })` | Delete data |

### **Route Parameters**

```js
app.get("/users/:id", (req, res) => {
  const userId = req.params.id; // Access route params
  res.send(`User ID: ${userId}`);
});
```

### **Query Parameters**

```js
app.get("/search", (req, res) => {
  const query = req.query.q; // ?q=express
  res.send(`Searching for: ${query}`);
});
```

---

## **3. Middleware**

### **Built-in Middleware**

```js
app.use(express.json()); // Parse JSON bodies
app.use(express.urlencoded({ extended: true })); // Parse URL-encoded data
app.use(express.static("public")); // Serve static files
```

### **Custom Middleware**

```js
const logger = (req, res, next) => {
  console.log(`${req.method} ${req.url}`);
  next(); // Pass control to the next middleware
};

app.use(logger);
```

### **Error Handling Middleware**

```js
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).send("Something broke!");
});
```

---

## **4. REST API Example**

```js
const users = [
  { id: 1, name: "John" },
  { id: 2, name: "Jane" },
];

// GET all users
app.get("/users", (req, res) => {
  res.json(users);
});

// GET a single user
app.get("/users/:id", (req, res) => {
  const user = users.find((u) => u.id === parseInt(req.params.id));
  if (!user) return res.status(404).send("User not found");
  res.json(user);
});

// POST a new user
app.post("/users", (req, res) => {
  const user = { id: users.length + 1, name: req.body.name };
  users.push(user);
  res.status(201).json(user);
});
```

---

## **5. Advanced Features**

### **Router (Modular Routes)**

```js
// routes/users.js
const express = require("express");
const router = express.Router();

router.get("/", (req, res) => {
  res.send("List of users");
});

module.exports = router;

// In main app.js
const userRoutes = require("./routes/users");
app.use("/users", userRoutes);
```

### **CORS Handling**

```bash
npm install cors
```

```js
const cors = require("cors");
app.use(cors()); // Enable CORS for all routes
```

### **File Uploads (Multer)**

```bash
npm install multer
```

```js
const multer = require("multer");
const upload = multer({ dest: "uploads/" });

app.post("/upload", upload.single("file"), (req, res) => {
  res.send("File uploaded!");
});
```

---

## **6. Authentication (JWT Example)**

```bash
npm install jsonwebtoken bcryptjs
```

```js
const jwt = require("jsonwebtoken");
const bcrypt = require("bcryptjs");

// Login route
app.post("/login", async (req, res) => {
  const { username, password } = req.body;
  // Validate user (pseudo-code)
  const user = await User.findOne({ username });
  if (!user) return res.status(400).send("Invalid credentials");

  const validPass = await bcrypt.compare(password, user.password);
  if (!validPass) return res.status(400).send("Invalid credentials");

  const token = jwt.sign({ _id: user._id }, "your-secret-key");
  res.header("auth-token", token).send(token);
});

// Protected route
const auth = (req, res, next) => {
  const token = req.header("auth-token");
  if (!token) return res.status(401).send("Access denied");

  try {
    const verified = jwt.verify(token, "your-secret-key");
    req.user = verified;
    next();
  } catch (err) {
    res.status(400).send("Invalid token");
  }
};

app.get("/protected", auth, (req, res) => {
  res.send("Protected data");
});
```

---

## **7. Deployment**

### **PM2 (Production Process Manager)**

```bash
npm install pm2 -g
pm2 start app.js --name "my-api"
pm2 save
pm2 startup
```

---

## **8. Credential Security**

### **Environment Variables (dotenv)**

```bash
npm install dotenv
```

```js
require("dotenv").config();
const PORT = process.env.PORT || 3000;
```

---

## **9. Best Practices**

- **Use `helmet` for security** (`npm install helmet`)
- **Validate input with `joi` or `express-validator`**
- **Use `morgan` for logging** (`npm install morgan`)
- **Structure your app modularly (routes, controllers, models)**

---

### **Quick Start Command**

```bash
npm init -y && npm install express cors morgan helmet && npm install --save-dev nodemon
```
