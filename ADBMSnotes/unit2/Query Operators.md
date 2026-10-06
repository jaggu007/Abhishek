In MongoDB, **Query Operators** are special operators (starting with `$`) used to filter documents when retrieving data.

## 1. Comparison Operators

Used to compare values.

| Operator                                                                    | Meaning | Example |
| --------------------------------------------------------------------------- | ------- | ------- |
| `$eq`  | Equal to                    | `{age: {$eq: 20}}`               |         |         |
| `$ne`  | Not equal to                | `{age: {$ne: 20}}`               |         |         |
| `$gt`  | Greater than                | `{age: {$gt: 20}}`               |         |         |
| `$gte` | Greater than or equal to    | `{age: {$gte: 20}}`              |         |         |
| `$lt`  | Less than                   | `{age: {$lt: 20}}`               |         |         |
| `$lte` | Less than or equal to       | `{age: {$lte: 20}}`              |         |         |
| `$in`  | Matches any value in a list | `{branch: {$in: ["CSE","IT"]}}`  |         |         |
| `$nin` | Not in a list               | `{branch: {$nin: ["CSE","IT"]}}` |         |         |

### Example

```javascript
db.students.find(
    { age: { $gt: 18 } }
)
```

Find students older than 18.

---

## 2. Logical Operators

Combine multiple conditions.

| Operator | Meaning                             |
| -------- | ----------------------------------- |
| `$and` | All conditions must be true         |
| `$or`  | At least one condition must be true |
| `$not` | Negates a condition                 |
| `$nor` | None of the conditions must be true |

### Example

```javascript
db.students.find({
    $and: [
        { branch: "CSE" },
        { age: { $gt: 18 } }
    ]
})
```

### Example

```javascript
db.students.find({
    $or: [
        { branch: "CSE" },
        { branch: "IT" }
    ]
})
```

---

## 3. Element Operators

Check whether fields exist or have a specific type.

| Operator    | Meaning                         |
| ----------- | ------------------------------- |
| `$exists` | Field exists                    |
| `$type`   | Field is of specified BSON type |

### Example

```javascript
db.students.find({
    email: { $exists: true }
})
```

---

## 4. Evaluation Operators

| Operator   | Meaning           |
| ---------- | ----------------- |
| `$regex` | Pattern matching  |
| `$text`  | Text search       |
| `$mod`   | Modulus operation |

### Example

```javascript
db.students.find({
    name: { $regex: "^A" }
})
```

Names starting with "A".

---

## 5. Array Operators

| Operator       | Meaning                             |
| -------------- | ----------------------------------- |
| `$all`       | Array contains all specified values |
| `$size`      | Array size                          |
| `$elemMatch` | Match array elements                |

### Example

```javascript
db.students.find({
    skills: { $all: ["Java", "MongoDB"] }
})
```

### Example

```javascript
db.students.find({
    skills: { $size: 3 }
})
```

---

## 6. Commonly Used Query Examples

### Find all students from CSE

```javascript
db.students.find({branch: "CSE"})
```

### Find students older than 20

```javascript
db.students.find({age: {$gt: 20}})
```

### Find students between 18 and 22

```javascript
db.students.find({
    age: {
        $gte: 18,
        $lte: 22
    }
})
```

### Find students in CSE or IT

```javascript
db.students.find({
    branch: {
        $in: ["CSE", "IT"]
    }
})
```

### Find students whose name starts with A

```javascript
db.students.find({
    name: {$regex: "^A"}
})
```

## Quick Memory Trick

* **Comparison Operators** → Compare values (`$gt`, `$lt`, `$eq`)
* **Logical Operators** → Combine conditions (`$and`, `$or`)
* **Element Operators** → Check fields (`$exists`)
* **Evaluation Operators** → Search patterns (`$regex`)
* **Array Operators** → Work with arrays (`$all`, `$size`)

The operators such as `$gt`, `$lt`, `$gte`, `$lte`, `$eq`, etc., are collectively called **MongoDB Query Operators** (more specifically,  **Comparison Query Operators** ).
