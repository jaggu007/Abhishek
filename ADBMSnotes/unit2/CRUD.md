Absolutely. For students beginning with MongoDB, I’d introduce the commands in this order—from  **starting `mongosh` → database → collection → CRUD → useful utilities** .

## 1. Start MongoDB Shell

Open Terminal/Command Prompt and run:

```bash
mongosh
```

Check the current connection:

```javascript
db
```

Show MongoDB server information:

```javascript
db.version()
```

Exit `mongosh`:

```javascript
exit
```

---

# 2. Database Commands

### Show all databases

```javascript
show dbs
```

### Create / switch to a database

```javascript
use KIET
```

> Important: `use KIET` does **not actually create** the database immediately. MongoDB creates it when you store some data in it.

Check current database:

```javascript
db
```

Example:

```javascript
use KIET
db
```

Output:

```text
KIET
```

## Create the database by inserting data

```javascript
use KIET

db.students.insertOne({
    name: "Rahul",
    age: 20,
    branch: "CSE"
})
```

Now:

```javascript
show dbs
```

will normally show `KIET`.

---

# 3. Collection Commands

MongoDB collections are roughly equivalent to **tables** in a relational database.

### Show collections

```javascript
show collections
```

### Create a collection

```javascript
db.createCollection("students")
```

Check:

```javascript
show collections
```

### Drop a collection

```javascript
db.students.drop()
```

⚠️ This permanently removes the collection and its documents.

---

# 4. Insert Documents

MongoDB stores data as  **documents** .

### Insert one document

```javascript
db.students.insertOne({
    name: "Rahul",
    age: 20,
    branch: "CSE"
})
```

### Insert multiple documents

```javascript
db.students.insertMany([
    {
        name: "Amit",
        age: 21,
        branch: "CSE"
    },
    {
        name: "Priya",
        age: 20,
        branch: "CSE-DS"
    },
    {
        name: "Neha",
        age: 22,
        branch: "IT"
    }
])
```

---

# 5. Read / Find Documents

This is one of the most important commands.

### Find all documents

```javascript
db.students.find()
```

### Display documents in readable format

```javascript
db.students.find().pretty()
```

### Find one document

```javascript
db.students.findOne()
```

### Find students from CSE

```javascript
db.students.find({
    branch: "CSE"
})
```

### Find students with age 20

```javascript
db.students.find({
    age: 20
})
```

### Find one specific student

```javascript
db.students.findOne({
    name: "Rahul"
})
```

---

# 6. Update Documents

### Update one document

```javascript
db.students.updateOne(
    { name: "Rahul" },
    { $set: { age: 21 } }
)
```

### Update multiple documents

For example, change the branch of all CSE students:

```javascript
db.students.updateMany(
    { branch: "CSE" },
    { $set: { department: "Computer Science" } }
)
```

### Increment a value

```javascript
db.students.updateOne(
    { name: "Rahul" },
    { $inc: { age: 1 } }
)
```

---

# 7. Delete Documents

### Delete one document

```javascript
db.students.deleteOne({
    name: "Rahul"
})
```

### Delete multiple documents

```javascript
db.students.deleteMany({
    branch: "CSE"
})
```

### Delete all documents

```javascript
db.students.deleteMany({})
```

⚠️ This removes all documents but  **does not remove the collection** .

---

# 8. Query Operators

These are particularly important for students.

### Greater than

Find students older than 20:

```javascript
db.students.find({
    age: { $gt: 20 }
})
```

### Greater than or equal

```javascript
db.students.find({
    age: { $gte: 20 }
})
```

### Less than

```javascript
db.students.find({
    age: { $lt: 21 }
})
```

### Less than or equal

```javascript
db.students.find({
    age: { $lte: 21 }
})
```

### Not equal

```javascript
db.students.find({
    branch: { $ne: "CSE" }
})
```

---

# 9. Logical Operators

### AND

```javascript
db.students.find({
    age: { $gt: 20 },
    branch: "CSE"
})
```

### OR

```javascript
db.students.find({
    $or: [
        { branch: "CSE" },
        { branch: "IT" }
    ]
})
```

### IN

Find students belonging to CSE or IT:

```javascript
db.students.find({
    branch: {
        $in: ["CSE", "IT"]
    }
})
```

---

# 10. Projection

Suppose you only want the student's name and branch.

```javascript
db.students.find(
    {},
    {
        name: 1,
        branch: 1
    }
)
```

Hide `_id`:

```javascript
db.students.find(
    {},
    {
        _id: 0,
        name: 1,
        branch: 1
    }
)
```

---

# 11. Sorting

### Ascending

```javascript
db.students.find().sort({
    age: 1
})
```

### Descending

```javascript
db.students.find().sort({
    age: -1
})
```

`1` → ascending
`-1` → descending

---

# 12. Limit

Display only 2 students:

```javascript
db.students.find().limit(2)
```

---

# 13. Count Documents

```javascript
db.students.countDocuments()
```

Count CSE students:

```javascript
db.students.countDocuments({
    branch: "CSE"
})
```

---

# 14. Useful `mongosh` Commands

These are good commands to introduce at the beginning of a practical class:

| Command                            | Purpose                               |
| ---------------------------------- | ------------------------------------- |
| `mongosh`                        | Open MongoDB Shell                    |
| `show dbs`                       | Display databases                     |
| `use KIET`                       | Switch/create target database context |
| `db`                             | Display current database              |
| `show collections`               | Display collections                   |
| `db.createCollection()`          | Create collection                     |
| `db.collection.insertOne()`      | Insert one document                   |
| `db.collection.insertMany()`     | Insert multiple documents             |
| `db.collection.find()`           | Find documents                        |
| `db.collection.findOne()`        | Find one document                     |
| `db.collection.updateOne()`      | Update one document                   |
| `db.collection.updateMany()`     | Update multiple documents             |
| `db.collection.deleteOne()`      | Delete one document                   |
| `db.collection.deleteMany()`     | Delete multiple documents             |
| `db.collection.countDocuments()` | Count documents                       |
| `db.collection.drop()`           | Delete collection                     |
| `db.version()`                   | Check MongoDB version                 |
| `exit`                           | Exit `mongosh`                      |

---

## ⭐ A Simple First Practical for Students

I would actually give students this **single sequence** first:

```javascript
mongosh

show dbs

use KIET

db

db.createCollection("students")

show collections

db.students.insertMany([
    {name: "Rahul", age: 20, branch: "CSE"},
    {name: "Amit", age: 21, branch: "CSE"},
    {name: "Priya", age: 20, branch: "CSE-DS"},
    {name: "Neha", age: 22, branch: "IT"}
])

db.students.find()

db.students.findOne()

db.students.find({branch: "CSE"})

db.students.find({age: {$gt: 20}})

db.students.updateOne(
    {name: "Rahul"},
    {$set: {age: 21}}
)

db.students.deleteOne({
    name: "Neha"
})

db.students.countDocuments()

show collections
```

This gives students the basic  **MongoDB CRUD cycle** :

**Create → Read → Update → Delete**

Once they are comfortable with this, the next logical step is  **MongoDB operators → projection → sorting → aggregation → indexes → transactions** .
