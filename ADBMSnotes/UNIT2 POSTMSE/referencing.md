# Understanding Referencing in MongoDB

After learning  **Embedding** , the next question is:

> If embedding means storing data *inside* a document, then what exactly is  *referencing* ?

The answer is simple:

> **Referencing means storing related data in separate documents/collections and connecting them using IDs.**

---

# Real-Life Analogy

Imagine a college.

Instead of keeping all information about a student in one file, the college maintains separate records:

### Student Record

```text
Student ID : 101
Name       : Rahul
```

### Course Record

```text
Course ID : 501
Course    : MongoDB
```

To know which course Rahul is studying, the college keeps:

```text
Student ID : 101
Course ID  : 501
```

Notice:

The course details are NOT inside the student record.

The student record simply **references** the course through an ID.

This is exactly how MongoDB referencing works.

---

# What is a Reference?

A reference is usually:

```text
_id value of another document
```

For example:

Student document:

```json
{
    "_id": 101,
    "name": "Rahul"
}
```

Course document:

```json
{
    "_id": 501,
    "courseName": "MongoDB"
}
```

Student stores:

```json
{
    "_id": 101,
    "name": "Rahul",
    "courseId": 501
}
```

The value:

```json
courseId : 501
```

points to another document.

Thus it is called a  **reference** .

---

# Why Do We Need Referencing?

Let's understand through a problem.

---

## Scenario: Student and Courses

Suppose Rahul studies:

* MongoDB
* Java
* Machine Learning

If we embed everything:

```json
{
    "_id": 101,
    "name": "Rahul",

    "courses": [
        "MongoDB",
        "Java",
        "Machine Learning"
    ]
}
```

Looks fine.

But now imagine:

```text
10,000 students
500 courses
```

Each course is taken by thousands of students.

---

If MongoDB course changes to:

```text
Advanced MongoDB
```

Then every student document containing:

```text
MongoDB
```

must be updated.

Huge problem.

---

Instead:

Store course information once.

### Courses Collection

```json
{
    "_id": 501,
    "courseName": "MongoDB"
}
```

### Students Collection

```json
{
    "_id": 101,
    "name": "Rahul",
    "courseIds": [501, 502, 503]
}
```

Now course information exists only once.

This is the major advantage of referencing.

---

# Visual Representation

## Embedding

```text
Student
│
├── Name
├── Address
├── Phones
└── Courses
      ├── MongoDB
      ├── Java
      └── ML
```

Everything is inside.

---

## Referencing

```text
Students Collection

101 Rahul
      │
      │
      ▼

Courses Collection

501 MongoDB
502 Java
503 ML
```

Documents are separate.

Only IDs connect them.

---

# Example 1: One-to-One Relationship

Consider Student and Library Card.

### Students Collection

```json
{
    "_id": 101,
    "name": "Rahul",
    "libraryCardId": 1001
}
```

### LibraryCards Collection

```json
{
    "_id": 1001,
    "issueDate": "2026-01-01",
    "expiryDate": "2027-01-01"
}
```

Student references Library Card.

---

# Example 2: One-to-Many Relationship

A customer places many orders.

---

### Customers Collection

```json
{
    "_id": 1,
    "name": "Abhishek"
}
```

### Orders Collection

```json
{
    "_id": 5001,
    "customerId": 1,
    "amount": 2000
}
```

```json
{
    "_id": 5002,
    "customerId": 1,
    "amount": 1500
}
```

The order references the customer.

---

# Example 3: Many-to-Many Relationship

This is where referencing becomes extremely useful.

---

## Students

```json
{
    "_id": 101,
    "name": "Rahul"
}
```

```json
{
    "_id": 102,
    "name": "Aman"
}
```

---

## Courses

```json
{
    "_id": 501,
    "course": "MongoDB"
}
```

```json
{
    "_id": 502,
    "course": "Java"
}
```

---

## Enrollments

```json
{
    "studentId": 101,
    "courseId": 501
}
```

```json
{
    "studentId": 101,
    "courseId": 502
}
```

```json
{
    "studentId": 102,
    "courseId": 501
}
```

This structure handles thousands of students and courses efficiently.

---

# How Do We Retrieve Referenced Data?

MongoDB provides:

```javascript
$lookup
```

which is similar to SQL JOIN.

---

## Example

Students

```json
{
    "_id": 101,
    "name": "Rahul",
    "courseId": 501
}
```

Courses

```json
{
    "_id": 501,
    "courseName": "MongoDB"
}
```

Query:

```javascript
db.students.aggregate([
{
    $lookup: {
        from: "courses",
        localField: "courseId",
        foreignField: "_id",
        as: "courseDetails"
    }
}
])
```

Result:

```json
{
    "_id": 101,
    "name": "Rahul",
    "courseId": 501,

    "courseDetails": [
        {
            "_id": 501,
            "courseName": "MongoDB"
        }
    ]
}
```

MongoDB joins the collections when needed.

---

# Advantages of Referencing

### 1. Avoids Data Duplication

Store data once.

```text
Course stored once
```

instead of thousands of times.

---

### 2. Easier Updates

Change course name once.

Every student automatically sees the updated course name.

---

### 3. Better for Large Data

Examples:

* Social media comments
* Orders
* Transactions
* Chat messages
* Sensor records

---

### 4. Prevents Huge Documents

MongoDB document size limit:

```text
16 MB
```

Referencing helps avoid hitting this limit.

---

# Disadvantages of Referencing

### 1. Additional Queries

Need:

```javascript
$lookup
```

or multiple queries.

---

### 2. Slower Reads

MongoDB must access multiple collections.

---

### 3. More Complex Design

Relationships must be managed manually.

---

# When Should We Use Referencing?

Use referencing when:

### Large Related Data

```text
Customer → Orders
Post → Comments
Teacher → Students
```

---

### Many-to-Many Relationships

```text
Students ↔ Courses
Doctors ↔ Patients
Actors ↔ Movies
```

---

### Frequently Updated Shared Data

```text
Course Information
Product Information
Department Information
```

---

# Embedding vs Referencing

| Feature          | Embedding       | Referencing        |
| ---------------- | --------------- | ------------------ |
| Storage          | Inside document | Separate documents |
| Read Speed       | Fast            | Slower             |
| Updates          | Harder          | Easier             |
| Data Duplication | More            | Less               |
| Joins Needed     | No              | Yes ($lookup)      |
| Large Data       | Not suitable    | Suitable           |
| Complexity       | Simple          | More complex       |

---

# Simple Memory Trick

Imagine a student and his address.

### Embedding

```text
Student File
 └── Address inside file
```

Everything is physically together.

---

### Referencing

```text
Student File
 └── Address File Number = 2001
```

Student file only stores a pointer (ID).

To get the address, you must open the address file separately.

---
