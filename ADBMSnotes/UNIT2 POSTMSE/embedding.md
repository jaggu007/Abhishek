# Understanding Embedding in MongoDB

Many students initially think:

> "This is just a JSON object inside another JSON object. Why is it called  *embedding* ?"

That's actually the right question.

---

# What Does "Embed" Mean?

In simple English:

**Embed = To place something inside something else.**

Examples:

* Embed a photo in a Word document.
* Embed a YouTube video in a webpage.
* Embed an object inside another object.

MongoDB uses the same idea.

Instead of storing related data in separate collections, we  **place (embed) the related data inside the parent document itself** .

---

# Example 1: Student and Address

Suppose we have:

### Student

```json
{
    "_id": 101,
    "name": "Rahul"
}
```

### Address

```json
{
    "city": "Delhi",
    "pincode": 110001
}
```

Instead of keeping them separately, MongoDB allows:

```json
{
    "_id": 101,
    "name": "Rahul",

    "address": {
        "city": "Delhi",
        "pincode": 110001
    }
}
```

Notice:

```json
"address": {
    ...
}
```

The address document is stored **inside** the student document.

Therefore:

**Address is embedded inside Student.**

---

# Visual Representation

```text
Student Document
│
├── _id : 101
├── name : Rahul
│
└── address
      │
      ├── city : Delhi
      └── pincode : 110001
```

Address is physically part of the same document.

---

# Why Not Store Separately?

In a relational database:

### Student Table

| StudentID | Name  |
| --------- | ----- |
| 101       | Rahul |

### Address Table

| StudentID | City  | Pincode |
| --------- | ----- | ------- |
| 101       | Delhi | 110001  |

To get complete information:

```sql
SELECT *
FROM Student s
JOIN Address a
ON s.StudentID = a.StudentID;
```

A JOIN is needed.

MongoDB tries to avoid frequent joins.

So it stores:

```json
Student + Address
```

in a single document.

---

# Example 2: Student with Multiple Phone Numbers

A student can have several phone numbers.

### Embedded Version

```json
{
    "_id": 101,
    "name": "Rahul",

    "phones": [
        "9876543210",
        "9123456789"
    ]
}
```

The phone numbers are stored inside the student document.

Thus:

**Phones are embedded.**

---

# Why is This Beneficial?

Imagine the query:

```javascript
db.students.findOne({_id:101})
```

MongoDB returns:

```json
{
    "_id": 101,
    "name": "Rahul",

    "phones": [
        "9876543210",
        "9123456789"
    ]
}
```

Everything comes back in  **one read operation** .

No lookup.
No join.
No second query.

This is the biggest advantage of embedding.

---

# Real-Life Analogy: School File

Think of a student's physical file.

### Embedded Approach

```text
Student File
│
├── Personal Details
├── Address
├── Phone Numbers
├── Parent Details
└── Attendance
```

Everything is in one folder.

Open the folder once → all information is available.

This is embedding.

---

# Example 3: Parent Details

```json
{
    "_id": 101,
    "name": "Rahul",

    "parent": {
        "father": "Rajesh",
        "mother": "Sunita",
        "phone": "9999999999"
    }
}
```

The parent information is embedded because it is stored inside the student document.

---

# Types of Embedding

## 1. Single Embedded Document

```json
{
    "name": "Rahul",
    "address": {
        "city": "Delhi",
        "pincode": 110001
    }
}
```

Here:

```json
address
```

is one embedded document.

---

## 2. Array of Embedded Documents

```json
{
    "name": "Rahul",

    "addresses": [
        {
            "city": "Delhi"
        },
        {
            "city": "Noida"
        }
    ]
}
```

Each object inside the array is an embedded document.

---

# How MongoDB Stores It Internally

MongoDB stores the entire document as one BSON document.

```text
Student Document
│
├── Name
├── Branch
├── Address
├── Phones
└── Parent Details
```

Everything is stored together.

When MongoDB fetches the document, all embedded information comes along automatically.

---

# When Should We Use Embedding?

Use embedding when:

### 1. Data belongs to one parent

Example:

```text
Student → Address
Student → Parent
Student → Phone Numbers
```

---

### 2. Data is frequently accessed together

If every time you fetch a student, you also need the address, keep them together.

---

### 3. Small amount of related data

Good:

```text
Student → 2 Addresses
Student → 3 Phone Numbers
```

Bad:

```text
Student → 10 lakh attendance records
```

The document becomes huge.

---

# When Should We Avoid Embedding?

Suppose a Facebook post has:

```text
50 lakh comments
```

Embedding all comments:

```json
{
    "post":"Hello",
    "comments":[
        ...
        50 lakh comments ...
    ]
}
```

Problems:

* Very large document
* Slow updates
* May exceed MongoDB's 16 MB document limit

Here, referencing is better.

---

# Rule of Thumb 

> **Embedding means storing related data directly inside a parent document as nested documents or arrays, so that related information can be retrieved in a single database operation without using joins or lookups.**
