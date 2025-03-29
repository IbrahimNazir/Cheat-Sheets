# **MongoDB Cheat Sheet**

## **1. MongoDB Basics**

### **Installation & Setup**

```bash
# Install MongoDB Community Edition (Ubuntu)
sudo apt-get install mongodb

# Start MongoDB service
sudo systemctl start mongod

# Connect to MongoDB Shell
mongosh

# Check MongoDB version
mongod --version
```

### **Basic Commands**

| Command                        | Description                    |
| ------------------------------ | ------------------------------ |
| `show dbs`                     | List all databases             |
| `use <db_name>`                | Switch/Create database         |
| `db.createCollection("users")` | Create a collection            |
| `show collections`             | List collections in current DB |
| `db.dropDatabase()`            | Delete current database        |

---

## **2. CRUD Operations**

### **Insert Documents**

```javascript
// Insert one
db.users.insertOne({
  name: "John Doe",
  age: 30,
  email: "john@example.com",
});

// Insert many
db.users.insertMany([
  { name: "Alice", age: 25 },
  { name: "Bob", age: 28 },
]);
```

### **Query Documents**

| Query                      | Example                                  |
| -------------------------- | ---------------------------------------- |
| Find all                   | `db.users.find()`                        |
| Find with filter           | `db.users.find({ age: { $gt: 25 } })`    |
| Find one                   | `db.users.findOne({ name: "John" })`     |
| Projection (select fields) | `db.users.find({}, { name: 1, age: 1 })` |

### **Update Documents**

```javascript
// Update one
db.users.updateOne({ name: "John" }, { $set: { age: 31 } });

// Update many
db.users.updateMany(
  { age: { $lt: 30 } },
  { $inc: { age: 1 } } // Increment age by 1
);

// Replace document
db.users.replaceOne({ name: "John" }, { name: "John Doe", age: 32 });
```

### **Delete Documents**

```javascript
// Delete one
db.users.deleteOne({ name: "John" });

// Delete many
db.users.deleteMany({ age: { $gt: 30 } });
```

---

## **3. Query Operators**

### **Comparison Operators**

| Operator | Example                                     |
| -------- | ------------------------------------------- |
| `$eq`    | `db.users.find({ age: { $eq: 25 } })`       |
| `$ne`    | `db.users.find({ age: { $ne: 25 } })`       |
| `$gt`    | `db.users.find({ age: { $gt: 25 } })`       |
| `$lt`    | `db.users.find({ age: { $lt: 25 } })`       |
| `$in`    | `db.users.find({ age: { $in: [25, 30] } })` |

### **Logical Operators**

| Operator | Example                                                     |
| -------- | ----------------------------------------------------------- |
| `$and`   | `db.users.find({ $and: [{ age: 25 }, { name: "Alice" }] })` |
| `$or`    | `db.users.find({ $or: [{ age: 25 }, { name: "Bob" }] })`    |
| `$not`   | `db.users.find({ age: { $not: { $gt: 25 } } })`             |

### **Array Operators**

| Operator     | Example                                                                  |
| ------------ | ------------------------------------------------------------------------ |
| `$push`      | `db.users.updateOne({ name: "John" }, { $push: { hobbies: "coding" } })` |
| `$pull`      | `db.users.updateOne({ name: "John" }, { $pull: { hobbies: "coding" } })` |
| `$elemMatch` | `db.users.find({ hobbies: { $elemMatch: { $eq: "coding" } } })`          |

---

## **4. Indexing & Performance**

### **Create Indexes**

```javascript
// Single field index
db.users.createIndex({ email: 1 }); // 1 = ascending, -1 = descending

// Compound index
db.users.createIndex({ name: 1, age: -1 });

// Text index (for search)
db.users.createIndex({ name: "text" });

// Unique index
db.users.createIndex({ email: 1 }, { unique: true });
```

### **Check & Drop Indexes**

```javascript
// List indexes
db.users.getIndexes();

// Drop index
db.users.dropIndex("email_1");
```

### **Query Optimization**

- Use `explain()` to analyze queries:
  ```javascript
  db.users.find({ age: { $gt: 25 } }).explain("executionStats");
  ```
- Covered queries (use indexes only):
  ```javascript
  db.users.find({ email: "john@example.com" }, { _id: 0, email: 1 });
  ```

---

## **5. Aggregation Framework**

### **Basic Aggregation**

```javascript
// Group by age and count
db.users.aggregate([{ $group: { _id: "$age", count: { $sum: 1 } } }]);

// Match + Group
db.users.aggregate([
  { $match: { age: { $gt: 25 } } },
  { $group: { _id: "$name", total: { $sum: 1 } } },
]);
```

### **Pipeline Stages**

| Stage      | Example          |
| ---------- | ---------------- |
| `$match`   | Filter documents |
| `$group`   | Group by field   |
| `$sort`    | Sort results     |
| `$project` | Select fields    |
| `$limit`   | Limit results    |
| `$lookup`  | Join collections |

### **$lookup (Join Collections)**

```javascript
db.orders.aggregate([
  {
    $lookup: {
      from: "users",
      localField: "userId",
      foreignField: "_id",
      as: "userDetails",
    },
  },
]);
```

---

## **6. Transactions**

```javascript
const session = db.getMongo().startSession();
session.startTransaction();

try {
  db.users.insertOne({ name: "Alice" }, { session });
  db.orders.insertOne({ userId: 1, product: "Book" }, { session });
  session.commitTransaction();
} catch (err) {
  session.abortTransaction();
  console.error("Transaction aborted:", err);
} finally {
  session.endSession();
}
```

---

## **7. Security & Best Practices**

### **User Management**

```javascript
// Create admin user
use admin
db.createUser({
  user: "admin",
  pwd: "password123",
  roles: ["root"]
});

// Enable authentication in `mongod.conf`:
security:
  authorization: enabled
```

### **Backup & Restore**

```bash
# Backup database
mongodump --db mydb --out /backup

# Restore database
mongorestore --db mydb /backup/mydb
```

### **Best Practices**

- **Use indexes wisely** (avoid over-indexing)
- **Enable authentication**
- **Use `$project` to limit returned fields**
- **Avoid large transactions**
- **Monitor performance with `mongostat`**

---

## **8. MongoDB with Node.js (Mongoose)**

### **Basic Setup**

```javascript
const mongoose = require("mongoose");
mongoose.connect("mongodb://localhost:27017/mydb");

const User = mongoose.model("User", {
  name: String,
  age: Number,
});

// Create user
const user = new User({ name: "John", age: 30 });
user.save().then(() => console.log("User saved!"));
```

### **Mongoose CRUD**

```javascript
// Find
User.find({ age: { $gt: 25 } });

// Update
User.updateOne({ name: "John" }, { age: 31 });

// Delete
User.deleteOne({ name: "John" });
```

---

### **Quick Reference**

- **MongoDB Shell**: `mongosh`
- **GUI Clients**: MongoDB Compass, Robo 3T
- **Cloud Atlas**: [https://www.mongodb.com/cloud/atlas](https://www.mongodb.com/cloud/atlas)
