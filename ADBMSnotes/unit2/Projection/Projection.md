### In MongoDB

Projection is used to **select which fields should be returned** in the query result.

**Example:**

```javascript
db.students.find(
    {},
    { name: 1, branch: 1, _id: 0 }
)
```

**Meaning:**

* `1` → include the field
* `0` → exclude the field
* `_id` is included by default, so `_id: 0` removes it

**Sample Document**

```javascript
{
    "_id": 1,
    "name": "Rahul",
    "branch": "CSE",
    "age": 20
}
```

**Output**

```javascript
{
    "name": "Rahul",
    "branch": "CSE"
}
```

### In Relational Databases (SQL)

Projection refers to selecting specific columns from a table.

```sql
SELECT name, branch
FROM students;
```

This is equivalent to MongoDB projection.

### Why use Projection?

* Reduces data transfer
* Improves query performance
* Returns only the required fields
* Makes applications more efficient

### Common MongoDB Projection Examples

**Include fields**

```javascript
db.students.find({}, { name: 1, age: 1, _id: 0 })
```

**Exclude fields**

```javascript
db.students.find({}, { marks: 0 })
```

**Projection with condition**

```javascript
db.students.find(
    { branch: "CSE" },
    { name: 1, cgpa: 1, _id: 0 }
)
```

# MongoDB Projection Rules

Projection controls **which fields are returned** by a query. MongoDB follows a few important rules when using projections.

---

## Rule 1: Inclusion Projection (`1`)

Use `1` to include fields.

```javascript
db.students.find(
    {},
    { name: 1, branch: 1 }
)
```

### Sample Document

```javascript
{
    "_id": 1,
    "name": "Rahul",
    "branch": "CSE",
    "age": 20,
    "cgpa": 8.5
}
```

### Output

```javascript
{
    "_id": 1,
    "name": "Rahul",
    "branch": "CSE"
}
```

### Observation

Even though `_id` was not specified, MongoDB includes it automatically.

---

## Rule 2: Exclusion Projection (`0`)

Use `0` to exclude fields.

```javascript
db.students.find(
    {},
    { age: 0, cgpa: 0 }
)
```

### Output

```javascript
{
    "_id": 1,
    "name": "Rahul",
    "branch": "CSE"
}
```

All fields are returned except those marked with `0`.

---

## Rule 3: `_id` is Included by Default

MongoDB always returns `_id` unless explicitly excluded.

```javascript
db.students.find(
    {},
    { name: 1, branch: 1, _id: 0 }
)
```

### Output

```javascript
{
    "name": "Rahul",
    "branch": "CSE"
}
```

---

## Rule 4: Cannot Mix Inclusion and Exclusion

❌ Invalid:

```javascript
db.students.find(
    {},
    { name: 1, age: 0 }
)
```

MongoDB Error:

```text
Cannot do exclusion on field age in inclusion projection
```

### Why?

MongoDB cannot determine whether the projection is:

* Inclusion-based, or
* Exclusion-based

Choose one style only.

---

## Rule 5: Exception for `_id`

You may include fields and exclude `_id` together.

✅ Valid:

```javascript
db.students.find(
    {},
    { name: 1, branch: 1, _id: 0 }
)
```

This is the only allowed mix.

---

## Rule 6: Projection Works After Filtering

Query Process:

```
Step 1 → Filter documents
Step 2 → Apply projection
Step 3 → Return result
```

Example:

```javascript
db.students.find(
    { branch: "CSE" },
    { name: 1, cgpa: 1, _id: 0 }
)
```

MongoDB:

1. Finds all CSE students.
2. Keeps only `name` and `cgpa`.
3. Returns the result.

---

## Rule 7: Project Nested Fields Using Dot Notation

Document:

```javascript
{
    "name": "Rahul",
    "address": {
        "city": "Delhi",
        "state": "Delhi"
    }
}
```

Query:

```javascript
db.students.find(
    {},
    { "address.city": 1, _id: 0 }
)
```

Output:

```javascript
{
    "address": {
        "city": "Delhi"
    }
}
```

---

## Rule 8: Project Specific Array Elements with `$slice`

Document:

```javascript
{
    "name": "Rahul",
    "marks": [78, 82, 90, 88, 91]
}
```

First 3 elements:

```javascript
db.students.find(
    {},
    { marks: { $slice: 3 } }
)
```

Output:

```javascript
{
    "marks": [78, 82, 90]
}
```

Last 2 elements:

```javascript
db.students.find(
    {},
    { marks: { $slice: -2 } }
)
```

Output:

```javascript
{
    "marks": [88, 91]
}
```

---

## Rule 9: Use Projection to Reduce Network Traffic

Without projection:

```javascript
db.students.find({})
```

Returns all fields.

With projection:

```javascript
db.students.find(
    {},
    { name: 1, branch: 1, _id: 0 }
)
```

Returns only required fields, making queries faster and reducing data transfer.

---

## Quick Revision Table

| Rule | Description                                                     | Example                      |
| ---- | --------------------------------------------------------------- | ---------------------------- |
| 1    | Include fields using `1`                                      | `{name:1}`                 |
| 2    | Exclude fields using `0`                                      | `{age:0}`                  |
| 3    | `_id`included by default                                      | `{name:1}`                 |
| 4    | Don't mix `1`and `0`                                        | `{name:1, age:0}`❌        |
| 5    | `_id`can be excluded with inclusion                           | `{name:1, _id:0}`✅        |
| 6    | Filtering happens before projection                             | `find(filter, projection)` |
| 7    | Use dot notation for nested fields                              | `{"address.city":1}`       |
| 8    | Use `$slice`for arrays               | `{marks:{$slice:3}}` |                              |
| 9    | Projection improves efficiency                                  | Return only needed fields    |


# Nested Field Projection in MongoDB

Nested field projection allows you to return **specific fields from an embedded document** instead of the entire embedded document.

---

## Example 1: Basic Nested Field Projection

### Document

```javascript
{
    "_id": 1,
    "name": "Rahul",
    "branch": "CSE",
    "address": {
        "city": "Delhi",
        "state": "Delhi",
        "pincode": 110001
    }
}
```

### Query

```javascript
db.students.find(
    {},
    { "address.city": 1, _id: 0 }
)
```

### Output

```javascript
{
    "address": {
        "city": "Delhi"
    }
}
```

Only the nested field `city` is returned.

---

## Example 2: Multiple Nested Fields

### Query

```javascript
db.students.find(
    {},
    {
        "address.city": 1,
        "address.state": 1,
        _id: 0
    }
)
```

### Output

```javascript
{
    "address": {
        "city": "Delhi",
        "state": "Delhi"
    }
}
```

---

## Example 3: Top-Level + Nested Fields

### Query

```javascript
db.students.find(
    {},
    {
        name: 1,
        "address.city": 1,
        _id: 0
    }
)
```

### Output

```javascript
{
    "name": "Rahul",
    "address": {
        "city": "Delhi"
    }
}
```

You can project top-level and nested fields together.

---

## Example 4: Excluding a Nested Field

### Document

```javascript
{
    "name": "Rahul",
    "address": {
        "city": "Delhi",
        "state": "Delhi",
        "pincode": 110001
    }
}
```

### Query

```javascript
db.students.find(
    {},
    { "address.pincode": 0 }
)
```

### Output

```javascript
{
    "_id": 1,
    "name": "Rahul",
    "address": {
        "city": "Delhi",
        "state": "Delhi"
    }
}
```

Everything except `address.pincode` is returned.

---

## Example 5: Deeply Nested Documents

### Document

```javascript
{
    "name": "Rahul",
    "college": {
        "department": {
            "name": "CSE",
            "hod": "Dr. Sharma"
        }
    }
}
```

### Query

```javascript
db.students.find(
    {},
    {
        "college.department.name": 1,
        _id: 0
    }
)
```

### Output

```javascript
{
    "college": {
        "department": {
            "name": "CSE"
        }
    }
}
```

---

# Real-World Practice Questions

### Question 1

Given:

```javascript
{
    "name": "Amit",
    "contact": {
        "email": "amit@gmail.com",
        "phone": "9876543210"
    }
}
```

Write a query to display only the email address.

**Answer**

```javascript
db.students.find(
    {},
    { "contact.email": 1, _id: 0 }
)
```

---

### Question 2

Given:

```javascript
{
    "name": "Priya",
    "academic": {
        "cgpa": 9.1,
        "semester": 5
    }
}
```

Display only name and CGPA.

**Answer**

```javascript
db.students.find(
    {},
    {
        name: 1,
        "academic.cgpa": 1,
        _id: 0
    }
)
```

---

### Question 3

Exclude the salary field from:

```javascript
{
    "name": "Ravi",
    "job": {
        "designation": "Developer",
        "salary": 60000
    }
}
```

**Answer**

```javascript
db.employees.find(
    {},
    { "job.salary": 0 }
)
```

---

# Important Exam Point

MongoDB uses **dot notation (`.`)** to access nested fields.

**Syntax:**

```javascript
{
    "parent.child": 1
}
```

Examples:

```javascript
{
    "address.city": 1
}
```

```javascript
{
    "contact.email": 1
}
```

```javascript
{
    "college.department.name": 1
}
```

### Memory Trick

Think of a nested document like a folder structure:

```text
address
 ├── city
 ├── state
 └── pincode
```

To access `city`, use:

```javascript
"address.city"
```

Just like a file path:

```text
Folder.SubFolder.File
```

MongoDB's dot notation works the same way.
