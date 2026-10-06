# Cursor Handling in MongoDB

## What is a Cursor?

A **Cursor** is an object returned by MongoDB when a query retrieves multiple documents.

Instead of sending all matching documents at once, MongoDB returns a **cursor** that points to the result set. The cursor allows documents to be fetched one by one or in batches.

### Simple Definition

> A Cursor is a pointer to the documents returned by a query.

Think of it like a bookmark in a book:

* The query finds all matching documents.
* The cursor keeps track of the current position.
* You move through the results using the cursor.

---

## Why Does MongoDB Use Cursors?

Imagine a collection contains  **1 million documents** .

If MongoDB returned all documents immediately:

* Huge memory consumption
* Slow network transfer
* Possible application crash

Instead:

1. MongoDB creates a cursor.
2. Documents are returned in small batches.
3. Additional documents are fetched only when needed.

---

## Basic Example

### Collection: Students

```javascript
db.students.insertMany([
    {name:"Aman", age:20},
    {name:"Riya", age:21},
    {name:"Karan", age:22}
])
```

### Query

```javascript
var cur = db.students.find()
```

Output:

```text
Cursor object created
```

`cur` now points to the query result.

---

## Viewing Documents Using Cursor

### Method 1: next()

Returns the next document.

```javascript
cur.next()
```

Output

```javascript
{ "_id":..., "name":"Aman", "age":20 }
```

Again:

```javascript
cur.next()
```

Output

```javascript
{ "_id":..., "name":"Riya", "age":21 }
```

---

### Method 2: hasNext()

Checks whether more documents exist.

```javascript
cur.hasNext()
```

Output

```text
true
```

or

```text
false
```

---

## Iterating Through All Documents

```javascript
var cur = db.students.find()

while(cur.hasNext())
{
    printjson(cur.next())
}
```

Output

```javascript
{
 "name":"Aman",
 "age":20
}
{
 "name":"Riya",
 "age":21
}
{
 "name":"Karan",
 "age":22
}
```

---

## Cursor Workflow

```text
find()
   │
   ▼
Cursor Created
   │
   ▼
hasNext()
   │
   ▼
next()
   │
   ▼
Move to Next Document
```

---

## Common Cursor Methods

| Method        | Purpose                                                  |
| ------------- | -------------------------------------------------------- |
| `hasNext()` | Checks if more documents exist                           |
| `next()`    | Returns next document                                    |
| `forEach()` | Iterates through all documents                           |
| `toArray()` | Converts result into array                               |
| `limit()`   | Restricts number of documents                            |
| `skip()`    | Skips documents                                          |
| `sort()`    | Sorts documents                                          |
| `count()`*  | Counts matching documents (*deprecated in some contexts) |
| `close()`   | Closes cursor manually                                   |

---

## Example: forEach()

```javascript
db.students.find().forEach(function(doc){
    print(doc.name)
})
```

Output

```text
Aman
Riya
Karan
```

---

## Example: toArray()

```javascript
db.students.find().toArray()
```

Output

```javascript
[
 {name:"Aman", age:20},
 {name:"Riya", age:21},
 {name:"Karan", age:22}
]
```

---

## Example: limit()

```javascript
db.students.find().limit(2)
```

Output

```javascript
Aman
Riya
```

Only first 2 documents are returned.

---

## Example: skip()

```javascript
db.students.find().skip(1)
```

Output

```javascript
Riya
Karan
```

First document is skipped.

---

## Example: sort()

```javascript
db.students.find().sort({age:-1})
```

Output

```javascript
Karan 22
Riya 21
Aman 20
```

`-1` → Descending order

`1` → Ascending order

---

## Real-World Use Case: Pagination

Suppose an e-commerce website displays:

```text
Page Size = 10 Products
```

### Page 1

```javascript
db.products.find()
           .skip(0)
           .limit(10)
```

### Page 2

```javascript
db.products.find()
           .skip(10)
           .limit(10)
```

### Page 3

```javascript
db.products.find()
           .skip(20)
           .limit(10)
```

Cursor helps retrieve only the required records instead of the entire collection.

---

## Advantages of Cursors

| Advantage           | Explanation                           |
| ------------------- | ------------------------------------- |
| Memory Efficient    | Documents fetched in batches          |
| Faster Retrieval    | No need to load entire result set     |
| Supports Pagination | Used with `skip()`and `limit()`   |
| Easy Traversal      | Iterate one document at a time        |
| Scalable            | Handles large collections efficiently |

---

## Exam Definition (2–3 Marks)

> A Cursor in MongoDB is a pointer returned by query operations such as `find()`. It allows the application to traverse query results document by document instead of loading all matching documents into memory at once. Common cursor methods include `next()`, `hasNext()`, `limit()`, `skip()`, and `sort()`.
>
