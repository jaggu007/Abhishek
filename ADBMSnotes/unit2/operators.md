# MongoDB Operators 

MongoDB operators are **special keywords beginning with `$`** that tell MongoDB  **what operation or condition to perform** .

For example:

```javascript
db.students.find({
    marks: {$gt: 80}
})
```

Here:

* `marks` → field
* `$gt` → MongoDB operator
* `80` → value
* `$gt` means **greater than**

So the query means:

> Find students whose marks are greater than 80.

---

# 1. Main Categories of MongoDB Operators

MongoDB operators can broadly be divided into:

```text
MongoDB Operators
│
├── 1. Comparison Operators
│
├── 2. Logical Operators
│
├── 3. Element Operators
│
├── 4. Evaluation Operators
│
├── 5. Array Operators
│
├── 6. Update Operators
│
└── 7. Miscellaneous Operators
```

For beginners, I recommend teaching them in this order:

**Comparison → Logical → Element → Array → Evaluation → Update**

---

# 2. Comparison Operators

These operators compare values.

### Important comparison operators

| Operator | Meaning                         |
| -------- | ------------------------------- |
| `$eq`  | Equal to                        |
| `$ne`  | Not equal to                    |
| `$gt`  | Greater than                    |
| `$gte` | Greater than or equal to        |
| `$lt`  | Less than                       |
| `$lte` | Less than or equal to           |
| `$in`  | Matches any value in a list     |
| `$nin` | Does not match values in a list |

Assume:

```javascript
db.students.find()
```

contains:

```javascript
{
    rollNo: 101,
    name: "Aman",
    branch: "CSE",
    age: 20,
    marks: 85
}
```

### `$gt`

Marks greater than 80:

```javascript
db.students.find({
    marks: {$gt: 80}
})
```

### `$gte`

Marks greater than or equal to 80:

```javascript
db.students.find({
    marks: {$gte: 80}
})
```

### `$lt`

Marks less than 40:

```javascript
db.students.find({
    marks: {$lt: 40}
})
```

### `$lte`

Marks less than or equal to 40:

```javascript
db.students.find({
    marks: {$lte: 40}
})
```

### `$ne`

Students whose branch is not CSE:

```javascript
db.students.find({
    branch: {$ne: "CSE"}
})
```

### `$in`

Students belonging to CSE or AIML:

```javascript
db.students.find({
    branch: {$in: ["CSE", "AIML"]}
})
```

### `$nin`

Students who are not from CSE or AIML:

```javascript
db.students.find({
    branch: {$nin: ["CSE", "AIML"]}
})
```

---

# 3. Logical Operators

Logical operators allow us to combine conditions.

The important ones are:

```text
$and
$or
$not
$nor
```

## `$and`

Find students who:

* have marks greater than 70
* AND are older than 18

```javascript
db.students.find({
    $and: [
        {marks: {$gt: 70}},
        {age: {$gt: 18}}
    ]
})
```

### Easier syntax

MongoDB usually allows us to write:

```javascript
db.students.find({
    marks: {$gt: 70},
    age: {$gt: 18}
})
```

Both conditions must be satisfied.

---

## `$or`

Find students who:

* have marks greater than 90
* OR belong to CSE

```javascript
db.students.find({
    $or: [
        {marks: {$gt: 90}},
        {branch: "CSE"}
    ]
})
```

---

## `$not`

Find students whose marks are  **not greater than 80** :

```javascript
db.students.find({
    marks: {$not: {$gt: 80}}
})
```

---

## `$nor`

Find students who are:

* not from CSE
* AND not from AIML

```javascript
db.students.find({
    $nor: [
        {branch: "CSE"},
        {branch: "AIML"}
    ]
})
```

---

# 4. Element Operators

These operators work with the  **existence and data type of fields** .

Main operators:

```text
$exists
$type
```

## `$exists`

Suppose some students have an `email` field and others don't.

Find students who have an email:

```javascript
db.students.find({
    email: {$exists: true}
})
```

Find students who don't have an email:

```javascript
db.students.find({
    email: {$exists: false}
})
```

---

## `$type`

Find documents where `age` is stored as an integer:

```javascript
db.students.find({
    age: {$type: "int"}
})
```

You can also check for strings:

```javascript
db.students.find({
    name: {$type: "string"}
})
```

---

# 5. Array Operators

MongoDB is particularly powerful with arrays.

Suppose we have:

```javascript
{
    name: "Aman",
    skills: ["Java", "MongoDB", "Python"]
}
```

Important array operators include:

```text
$all
$elemMatch
$size
```

## `$all`

Find students who have  **both Java and MongoDB** :

```javascript
db.students.find({
    skills: {
        $all: ["Java", "MongoDB"]
    }
})
```

---

## `$size`

Find students who have exactly 3 skills:

```javascript
db.students.find({
    skills: {$size: 3}
})
```

---

## `$elemMatch`

Suppose:

```javascript
{
    name: "Aman",
    subjects: [
        {name: "DBMS", marks: 85},
        {name: "Java", marks: 90}
    ]
}
```

Find students having a subject with marks greater than 85:

```javascript
db.students.find({
    subjects: {
        $elemMatch: {
            marks: {$gt: 85}
        }
    }
})
```

---

# 6. Evaluation Operators

These operators perform more specialized evaluations.

Important ones include:

```text
$regex
$text
$expr
$where
```

## `$regex`

Find students whose names start with `A`:

```javascript
db.students.find({
    name: {$regex: "^A"}
})
```

For example:

```text
Aman       ✓
Ankit      ✓
Rahul      ✗
Priya      ✗
```

Find names ending with `a`:

```javascript
db.students.find({
    name: {$regex: "a$"}
})
```

---

# 7. Update Operators

These are extremely important because they modify documents.

Common update operators:

```text
$set
$unset
$inc
$mul
$min
$max
$push
$pull
$addToSet
```

---

## `$set`

Change a field:

```javascript
db.students.updateOne(
    {rollNo: 101},
    {$set: {marks: 90}}
)
```

---

## `$inc`

Increase marks by 5:

```javascript
db.students.updateOne(
    {rollNo: 101},
    {$inc: {marks: 5}}
)
```

If marks were 85:

```text
85 + 5 = 90
```

---

## `$unset`

Remove a field:

```javascript
db.students.updateOne(
    {rollNo: 101},
    {$unset: {age: ""}}
)
```

The `age` field is removed.

---

## `$push`

Add an item to an array:

```javascript
db.students.updateOne(
    {rollNo: 101},
    {$push: {skills: "Java"}}
)
```

If:

```javascript
skills: ["Python", "MongoDB"]
```

becomes:

```javascript
skills: ["Python", "MongoDB", "Java"]
```

---

## `$addToSet`

Add an item only if it doesn't already exist:

```javascript
db.students.updateOne(
    {rollNo: 101},
    {$addToSet: {skills: "Java"}}
)
```

This prevents duplicate values.

---

## `$pull`

Remove an item from an array:

```javascript
db.students.updateOne(
    {rollNo: 101},
    {$pull: {skills: "Java"}}
)
```

---

# 8. Very Important: Operators Can Be Nested

This is where MongoDB queries become powerful.

For example:

```javascript
db.students.find({
    marks: {
        $gte: 60,
        $lte: 90
    }
})
```

means:

> Find students whose marks are between 60 and 90.

You can combine this with another condition:

```javascript
db.students.find({
    marks: {
        $gte: 60,
        $lte: 90
    },
    branch: {$in: ["CSE", "AIML"]}
})
```

Meaning:

> Find students whose marks are between 60 and 90 **AND** whose branch is either CSE or AIML.

---

# 9. Projection + Operators

You can also combine filtering with projection.

```javascript
db.students.find(
    {marks: {$gt: 80}},
    {name: 1, marks: 1, _id: 0}
)
```

Read this as:

> Find students with marks greater than 80, but display only their name and marks.

This is a very useful query to demonstrate to students because it combines  **filtering + comparison operator + projection** .

---

# 10. Quick Cheat Sheet

```text
COMPARISON
$eq       Equal
$ne       Not equal
$gt       Greater than
$gte      Greater than/equal
$lt       Less than
$lte      Less than/equal
$in       Matches any value
$nin      Doesn't match values

LOGICAL
$and      All conditions
$or       Any condition
$not      Negates condition
$nor      None of the conditions

ELEMENT
$exists   Field exists?
$type     Check data type

ARRAY
$all      Contains all values
$size     Array size
$elemMatch Match array element

EVALUATION
$regex    Pattern matching
$text     Text search
$expr     Compare expressions

UPDATE
$set      Set/change value
$unset    Remove field
$inc      Increment value
$mul      Multiply value
$push     Add to array
$pull     Remove from array
$addToSet Add unique value
```

### A good teaching sequence

For your MongoDB practical classes, I'd teach operators through one **`students` collection** rather than isolated examples:

**Create data → Comparison → Logical → Projection → Element → Array → Regex → Update operators → Combined queries.**

That way students see operators as actual tools for solving database problems, rather than just memorizing `$gt`, `$lt`, `$and`, etc.
