Perfect. **Element Operators** are used to check whether a field exists in a document or to determine the BSON data type of a field.

MongoDB provides two main Element Operators:

| Operator    | Purpose                              |
| ----------- | ------------------------------------ |
| `$exists` | Checks whether a field exists        |
| `$type`   | Checks the BSON data type of a field |

---

# 1. `$exists` Operator

Checks whether a field is present in a document.

## Sample Documents

```javascript
{
    student_id: "S001",
    name: "Aarav Sharma",
    age: 18,
    email: "aarav@gmail.com"
}

{
    student_id: "S002",
    name: "Ananya Gupta",
    age: 19
}
```

Notice that the second document has no `email` field.

---

## Example 1

Find students having an email field.

```javascript
db.students.find({
    email: {
        $exists: true
    }
})
```

---

## Example 2

Find students without an email field.

```javascript
db.students.find({
    email: {
        $exists: false
    }
})
```

---

## Practical Scenario

Suppose placement details are added only for placed students.

```javascript
{
    name: "Rahul",
    placementCompany: "Google"
}

{
    name: "Rohan"
}
```

Find students who have placement information.

```javascript
db.students.find({
    placementCompany: {
        $exists: true
    }
})
```

---

# 2. `$type` Operator

Checks the BSON datatype of a field.

---

## Common BSON Types

| Type    | Alias    |
| ------- | -------- |
| String  | "string" |
| Integer | "int"    |
| Double  | "double" |
| Boolean | "bool"   |
| Array   | "array"  |
| Object  | "object" |
| Date    | "date"   |

---

## Example Documents

```javascript
{
    name: "Aarav",
    age: 20,
    cgpa: 8.5
}
```

---

## Example 1

Find documents where name is a string.

```javascript
db.students.find({
    name: {
        $type: "string"
    }
})
```

---

## Example 2

Find documents where age is an integer.

```javascript
db.students.find({
    age: {
        $type: "int"
    }
})
```

---

## Example 3

Find documents where cgpa is stored as a double.

```javascript
db.students.find({
    cgpa: {
        $type: "double"
    }
})
```

---

# Practice Dataset Extension

Insert a few records containing additional fields:

```javascript
db.students.insertMany([
{
    student_id: "S101",
    name: "Amit",
    age: 20,
    email: "amit@gmail.com"
},
{
    student_id: "S102",
    name: "Neha",
    age: 21
},
{
    student_id: "S103",
    name: "Rohan",
    age: 22,
    skills: ["Java","MongoDB"]
}
])
```

---

# Practice Queries

### Easy

1. Find students having an email field.

```javascript
db.students.find({
    email: { $exists: true }
})
```

2. Find students without an email field.

```javascript
db.students.find({
    email: { $exists: false }
})
```

3. Find students having a skills field.
4. Find students without a skills field.

---

### Moderate

5. Find students whose age is stored as an integer.

```javascript
db.students.find({
    age: { $type: "int" }
})
```

6. Find students whose skills field is an array.

```javascript
db.students.find({
    skills: { $type: "array" }
})
```

7. Find students having both email and skills fields.

```javascript
db.students.find({
    email: { $exists: true },
    skills: { $exists: true }
})
```

---

### Advanced

8. Find students whose email exists but skills do not.

```javascript
db.students.find({
    email: { $exists: true },
    skills: { $exists: false }
})
```

9. Find documents where skills is an array and age is an integer.
10. Find students who have placementCompany information.

---

# Important Interview Question

### Difference Between `null` and `$exists:false`

Document:

```javascript
{
    name: "Rahul",
    email: null
}
```

This document **contains** the field `email`.

Therefore:

```javascript
db.students.find({
    email: {
        $exists: true
    }
})
```

✔ Returns the document.

But:

```javascript
db.students.find({
    email: {
        $exists: false
    }
})
```

✘ Does NOT return the document because the field exists, even though its value is `null`.

---

## Summary

| Operator           | Use                       |
| ------------------ | ------------------------- |
| `$exists:true`   | Field is present          |
| `$exists:false`  | Field is absent           |
| `$type:"string"` | Field contains a string   |
| `$type:"int"`    | Field contains an integer |
| `$type:"array"`  | Field contains an array   |
