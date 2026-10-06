# BSON vs JSON in MongoDB

MongoDB stores data internally in  **BSON (Binary JSON)** , while users typically work with  **JSON-like documents** .

## What is JSON?

**JSON (JavaScript Object Notation)** is a text-based format used to exchange data between applications.

Example:

```json
{
  "name": "Abhishek",
  "branch": "CSE",
  "year": 2
}
```

### Features of JSON

* Human-readable
* Text-based
* Lightweight
* Language-independent
* Commonly used in APIs and web applications

---

## What is BSON?

**BSON (Binary JSON)** is a binary-encoded format developed for MongoDB.

When you insert a JSON document into MongoDB:

```javascript
db.students.insertOne({
    name: "Abhishek",
    branch: "CSE",
    year: 2
})
```

MongoDB converts it internally into BSON before storing it on disk.

```text
JSON Document
      ↓
 BSON Conversion
      ↓
 Stored in MongoDB
```

---

## Why BSON?

JSON has limitations:

* No Date type
* No Binary type
* No ObjectId type
* Limited numeric types

BSON adds support for these MongoDB-specific data types.

Example BSON document:

```javascript
{
   "_id": ObjectId("68c1234abcd5678ef9012345"),
   "name": "Abhishek",
   "joiningDate": ISODate("2026-09-10"),
   "marks": NumberInt(95)
}
```

---

## JSON vs BSON Comparison

| Feature          | JSON           | BSON            |
| ---------------- | -------------- | --------------- |
| Format           | Text           | Binary          |
| Human Readable   | Yes            | No              |
| Storage Size     | Smaller        | Slightly Larger |
| Parsing Speed    | Slower         | Faster          |
| Date Support     | No Native Type | Yes             |
| Binary Data      | No             | Yes             |
| ObjectId Support | No             | Yes             |
| Used By          | APIs, Web Apps | MongoDB Storage |

---

## Example

### JSON

```json
{
  "name": "Rahul",
  "age": 21
}
```

### BSON Representation

```javascript
{
  "name": "Rahul",
  "age": NumberInt(21)
}
```

MongoDB stores the data in BSON but displays it in a JSON-like format for convenience.

---

## BSON Data Types

MongoDB supports many BSON types:

| Type        | Example            |
| ----------- | ------------------ |
| String      | `"Abhishek"`     |
| Integer     | `NumberInt(25)`  |
| Double      | `85.5`           |
| Boolean     | `true`           |
| Date        | `ISODate()`      |
| Array       | `[1,2,3]`        |
| Object      | `{city:"Delhi"}` |
| Null        | `null`           |
| ObjectId    | `ObjectId()`     |
| Binary Data | Binary Files       |

---

## Example with Different BSON Types

```javascript
db.students.insertOne({
    name: "Abhishek",
    age: 21,
    active: true,
    joiningDate: new Date(),
    skills: ["Java", "MongoDB"],
    address: {
        city: "Ghaziabad",
        state: "UP"
    }
})
```

MongoDB stores all of these using BSON types internally.

---

## Architecture Perspective

```text
Application
      ↓
JSON Document
      ↓
MongoDB Driver
      ↓
BSON Conversion
      ↓
MongoDB Storage Engine
      ↓
Disk
```

When data is retrieved:

```text
Disk
 ↓
BSON
 ↓
MongoDB Driver
 ↓
JSON-like Output
 ↓
Application
```

---

## Interview Question

**Q: Does MongoDB store JSON or BSON?**

**Answer:** MongoDB accepts JSON-like documents from users, but internally stores and processes them as  **BSON (Binary JSON)** .

### Quick Summary

* **JSON** = Human-readable text format for data exchange.
* **BSON** = Binary representation of JSON used internally by MongoDB.
* MongoDB  **receives JSON-like documents** , converts them to  **BSON** , stores them, and converts them back when displaying results.
