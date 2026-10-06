# Working with Arrays in MongoDB

Arrays are used when a field contains **multiple values** instead of a single value.

### Example Document

```json
{
    "_id": 1,
    "name": "Abhishek",
    "skills": ["Java", "MongoDB", "Python"],
    "marks": [85, 90, 78]
}
```

Here:

* `skills` is an array of strings.
* `marks` is an array of numbers.

---

# 1. Find Documents Containing a Value

### Find students who know Java

```javascript
db.students.find(
    { skills: "Java" }
)
```

MongoDB automatically checks whether `"Java"` exists inside the array.

### Result

```json
{
    "name": "Abhishek",
    "skills": ["Java", "MongoDB", "Python"]
}
```

---

# 2. Match Multiple Array Elements

Find students having both Java and MongoDB.

```javascript
db.students.find(
    { skills: { $all: ["Java", "MongoDB"] } }
)
```

### Why `$all`?

Because we want **all specified values** to be present.

---

# 3. Find Array Elements Using Comparison Operators

```javascript
db.students.find(
    { marks: { $gt: 90 } }
)
```

### Meaning

MongoDB checks whether **at least one element** in the array is greater than 90.

Example:

```json
"marks": [75, 82, 95]
```

Matches because `95 > 90`.

---

# 4. Find Documents by Array Size

Find students having exactly 3 skills.

```javascript
db.students.find(
    { skills: { $size: 3 } }
)
```

### Result

```json
{
    "name": "Abhishek",
    "skills": ["Java", "MongoDB", "Python"]
}
```

---

# 5. Access Specific Array Position

Arrays use  **zero-based indexing** .

```json
["Java", "MongoDB", "Python"]
```

| Index | Value   |
| ----- | ------- |
| 0     | Java    |
| 1     | MongoDB |
| 2     | Python  |

### Find where first skill is Java

```javascript
db.students.find(
    { "skills.0": "Java" }
)
```

---

# 6. Find Documents with Multiple Conditions on Array Elements

Suppose:

```json
{
    "marks": [75, 85, 95]
}
```

Find students having marks between 80 and 100.

```javascript
db.students.find(
    {
        marks: {
            $elemMatch: {
                $gte: 80,
                $lte: 100
            }
        }
    }
)
```

### Why `$elemMatch`?

Ensures the **same array element** satisfies all conditions.

---

# 7. Add an Element to an Array

### `$push`

```javascript
db.students.updateOne(
    { name: "Abhishek" },
    { $push: { skills: "React" } }
)
```

Before:

```json
["Java", "MongoDB", "Python"]
```

After:

```json
["Java", "MongoDB", "Python", "React"]
```

---

# 8. Add Multiple Elements

```javascript
db.students.updateOne(
    { name: "Abhishek" },
    {
        $push: {
            skills: {
                $each: ["NodeJS", "Express"]
            }
        }
    }
)
```

Result:

```json
["Java","MongoDB","Python","NodeJS","Express"]
```

---

# 9. Add Unique Values Only

### `$addToSet`

```javascript
db.students.updateOne(
    { name: "Abhishek" },
    { $addToSet: { skills: "Java" } }
)
```

If Java already exists, MongoDB does **not** add it again.

Think of `$addToSet` as an array version of a **Set** in Java.

---

# 10. Remove Elements from an Array

### `$pull`

```javascript
db.students.updateOne(
    { name: "Abhishek" },
    { $pull: { skills: "Python" } }
)
```

Before:

```json
["Java","MongoDB","Python"]
```

After:

```json
["Java","MongoDB"]
```

---

# 11. Remove Last Element

### `$pop`

```javascript
db.students.updateOne(
    { name: "Abhishek" },
    { $pop: { skills: 1 } }
)
```

Removes last element.

```json
["Java","MongoDB","Python"]
```

becomes

```json
["Java","MongoDB"]
```

---

# 12. Remove First Element

```javascript
db.students.updateOne(
    { name: "Abhishek" },
    { $pop: { skills: -1 } }
)
```

Removes first element.

---

# Quick Revision Table

| Operator       | Purpose                                    |
| -------------- | ------------------------------------------ |
| `$all`       | Match all specified values                 |
| `$size`      | Match array length                         |
| `$elemMatch` | Same element satisfies multiple conditions |
| `$push`      | Add element                                |
| `$each`      | Add multiple elements                      |
| `$addToSet`  | Add only if not already present            |
| `$pull`      | Remove matching element                    |
| `$pop: 1`    | Remove last element                        |
| `$pop: -1`   | Remove first element                       |

### Real-Life Analogy

Think of an array as a student's  **bag of skills** :

```json
"skills": ["Java", "MongoDB", "Python"]
```

* `$push` → Put a new book into the bag.
* `$pull` → Remove a book from the bag.
* `$addToSet` → Put a book only if it's not already there.
* `$size` → Count how many books are in the bag.
* `$all` → Check whether the bag contains all required books.
* `$elemMatch` → Check whether one specific book satisfies multiple conditions.
