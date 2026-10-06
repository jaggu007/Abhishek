# Data Modeling Patterns in MongoDB

Data modeling patterns are **reusable design solutions** that help structure MongoDB documents efficiently for performance, scalability, and maintainability.

Think of them as **best practices** for organizing data based on application requirements.

---

# 1. Embedding Pattern (Denormalization)

Store related data inside a single document.

### Example: Student and Address

```json
{
  "_id": 101,
  "name": "Abhishek",
  "branch": "CSE",
  "address": {
    "city": "Ghaziabad",
    "state": "UP",
    "pincode": 201206
  }
}
```

### Why Use It?

* Data is accessed together
* Faster reads
* No joins required
* Single query retrieves everything

### Real-Life Examples

* Student → Address
* Order → Order Items
* Blog → Comments (few comments)

### Advantages

✅ Fast retrieval

✅ Fewer queries

✅ Atomic updates

### Limitations

❌ Document size may grow

❌ Duplicate data possible

---

# 2. Referencing Pattern (Normalization)

Store related data in separate collections and connect them using IDs.

### Students Collection

```json
{
  "_id": 101,
  "name": "Abhishek",
  "departmentId": 1
}
```

### Departments Collection

```json
{
  "_id": 1,
  "department": "Computer Science"
}
```

### Why Use It?

* Large relationships
* Shared data
* Avoid duplication

### Advantages

✅ Smaller documents

✅ Easy updates

✅ No duplicate department data

### Limitations

❌ Additional query needed

❌ Slightly slower reads

---

# 3. Subset Pattern

Store only frequently used data in the main document and keep detailed data elsewhere.

### Example: E-Commerce Product

```json
{
  "_id": 1,
  "name": "Laptop",
  "price": 65000,
  "rating": 4.5
}
```

Detailed specifications:

```json
{
  "_id": 1,
  "processor": "i7",
  "ram": "16GB",
  "storage": "512GB SSD"
}
```

### Why?

Most users view:

* Name
* Price
* Rating

Rarely view:

* Technical specifications

### Benefit

Faster product listing pages.

---

# 4. Bucket Pattern

Group related records into a single document.

### Without Bucket

```json
{
  "sensorId": 1,
  "temperature": 28,
  "time": "10:00"
}
```

Thousands of such documents exist.

### With Bucket

```json
{
  "sensorId": 1,
  "date": "2026-10-01",
  "readings": [
    {"time":"10:00","temp":28},
    {"time":"10:05","temp":29},
    {"time":"10:10","temp":30}
  ]
}
```

### Used For

* IoT data
* Weather data
* Log data
* Time-series data

### Benefit

✅ Fewer documents

✅ Better storage efficiency

---

# 5. Computed Pattern

Store pre-calculated values.

### Example

Instead of calculating average rating every time:

```json
{
  "_id": 1,
  "product": "Laptop",
  "averageRating": 4.7
}
```

### Why?

Calculating repeatedly is expensive.

### Benefit

✅ Faster queries

### Trade-Off

❌ Need to update computed value when data changes.

---

# 6. Attribute Pattern

Store flexible attributes in an array.

### Example: Mobile Phones

```json
{
  "_id": 1,
  "name": "iPhone",
  "attributes": [
    {"key":"Color","value":"Black"},
    {"key":"RAM","value":"8GB"},
    {"key":"Storage","value":"256GB"}
  ]
}
```

### Why?

Different products have different attributes.

### Benefit

✅ Flexible schema

✅ Easy searching

---

# 7. Outlier Pattern

Handle unusually large records separately.

### Example

Most blog posts:

```json
{
  "title":"MongoDB Basics",
  "comments":[ ...20 comments... ]
}
```

One popular post:

```json
{
  "title":"MongoDB Advanced",
  "comments":[ ...50000 comments... ]
}
```

Store excessive comments in a separate collection.

### Benefit

Prevents huge documents from affecting performance.

---

# 8. Extended Reference Pattern

Combine embedding and referencing.

### Example

```json
{
  "_id": 101,
  "name": "Abhishek",
  "department": {
    "id": 1,
    "name": "CSE"
  }
}
```

Department collection:

```json
{
  "_id": 1,
  "name": "CSE"
}
```

### Why?

Frequently needed department name is embedded.

Full department details remain separate.

### Benefit

✅ Faster reads

✅ Reduced joins

---

# Quick Revision Table

| Pattern            | Purpose                     | Example                |
| ------------------ | --------------------------- | ---------------------- |
| Embedding          | Store related data together | Student + Address      |
| Referencing        | Avoid duplication           | Student → Department  |
| Subset             | Keep frequently used data   | Product List           |
| Bucket             | Group time-series records   | Sensor Readings        |
| Computed           | Store calculated values     | Average Rating         |
| Attribute          | Flexible fields             | Product Specifications |
| Outlier            | Handle huge records         | Viral Blog Comments    |
| Extended Reference | Mix embedding & reference   | Student + Dept Name    |


# **Q: When should we choose Embedding over Referencing?**

## Choose **Embedding** when:

* One-to-one or one-to-few relationship exists.
* Data is always accessed together.
* Data changes infrequently.

## Choose **Referencing** when:

* One-to-many or many-to-many relationship exists.
* Related data is shared by many documents.
* Data changes frequently.
* Document size may become very large.

A simple rule:

> **"Data accessed together → Embed. Data shared independently → Reference."**
>
