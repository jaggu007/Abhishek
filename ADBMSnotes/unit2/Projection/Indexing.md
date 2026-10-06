# MongoDB Indexing: Concepts and Index Types

## 1. What is an Index?

An **index** is a special data structure that MongoDB creates to make searching documents faster.

Think of a book.

Without an index:

> "Find all pages containing the word MongoDB."

You may have to scan every page.

With an index:

> "MongoDB → Pages 15, 28, 45, 72..."

You can directly jump to the required pages.

MongoDB works in a similar way.

## The Situation

Suppose we have the following collection:

```javascript
{
    _id: 1,
    name: "Rahul",
    age: 20
}

{
    _id: 2,
    name: "Amit",
    age: 20
}

{
    _id: 3,
    name: "Priya",
    age: 20
}

{
    _id: 4,
    name: "Neha",
    age: 21
}
```

Now create an index:

```javascript
db.students.createIndex({age: 1})
```

Notice that  **age = 20 appears three times** .

---

# How MongoDB Stores the Index

MongoDB does **not** store only one document per age value.

Conceptually, the index looks like:

```text
Age Value     Document References
----------------------------------
20            -> Doc 1
20            -> Doc 2
20            -> Doc 3
21            -> Doc 4
```

Or you can imagine:

```text
20 -> [_id:1, _id:2, _id:3]
21 -> [_id:4]
```

The actual internal structure is more sophisticated (a B-Tree), but conceptually this is what's happening.

---

# Query Example

When you run:

```javascript
db.students.find({age: 20})
```

MongoDB:

### Without Index

```text
Scan Document 1
Scan Document 2
Scan Document 3
Scan Document 4
...
```

### With Index

```text
Look up age = 20 in index
        ↓
Find references:
    Doc1
    Doc2
    Doc3
        ↓
Fetch documents
        ↓
Return results
```

MongoDB returns all matching documents.

---

---

# What If We Want No Duplicates?

Then create a  **unique index** :

```javascript
db.students.createIndex(
    {email: 1},
    {unique: true}
)
```

Now:

```javascript
{
    email: "abc@gmail.com"
}
```

and

```javascript
{
    email: "abc@gmail.com"
}
```

cannot both exist.

MongoDB throws an error.

---

# Internally: B-Tree Structure

MongoDB indexes are implemented using a **B-Tree** (more precisely, a B+Tree-like structure in the storage engine).

Conceptually:

```text
                [20]
               /    \
         [18,19]   [20,21,22]
```

For duplicate values:

```text
20
 |
 +--> Record A
 |
 +--> Record B
 |
 +--> Record C
```

The tree node stores the key (`20`) and references to all matching records.

---

# A Performance Observation

Suppose:

```text
100,000 students
```

and

```text
80,000 students have age = 20
```

You create:

```javascript
db.students.createIndex({age:1})
```

The query:

```javascript
db.students.find({age:20})
```

may still need to fetch  **80,000 documents** .

The index helps MongoDB  **find them quickly** , but MongoDB still has to  **return all 80,000 records** .

So an index is most effective when the indexed field has good  **selectivity** .

---

## Selectivity Concept

### High Selectivity (Excellent Index)

```text
Email
```

Example:

```text
rahul@gmail.com
amit@gmail.com
priya@gmail.com
```

Almost every value is unique.

Index works extremely well.

---

### Low Selectivity (Less Effective)

```text
Gender
```

```text
Male
Female
```

Only two possible values.

An index on gender often provides limited benefit because each value matches a large portion of the collection.

---

### Without an index

```text
Collection
   ↓
Document 1
Document 2
Document 3
Document 4
Document 5
...
Document 100000
   ↓
Check each document
```

This is called a  **collection scan** .

### With an index

```text
             Index
              ↓
        age = 25
              ↓
     Matching document locations
              ↓
        Required documents
```

MongoDB can avoid examining every document.

---

# 2. Why Do We Need Indexes?

Suppose we have:

```javascript
db.students.find({age: 21})
```

If the collection contains 1,00,000 students and there is no index on `age`, MongoDB may need to examine many or all documents.

If we create:

```javascript
db.students.createIndex({age: 1})
```

MongoDB can use the index to locate students with `age = 21` much more efficiently.

### Main benefits

Indexes can improve:

* `find()` queries
* sorting
* range queries
* equality searches
* some aggregation operations
* uniqueness enforcement

---

# 3. The Basic Index Command

The most important command is:

```javascript
db.collection.createIndex({field: 1})
```

Example:

```javascript
db.students.createIndex({age: 1})
```

Here:

```text
age → field being indexed
1   → ascending order
```

For descending order:

```javascript
db.students.createIndex({age: -1})
```

---

# 4. Ascending vs Descending Index

MongoDB uses:

```javascript
1
```

for ascending order.

```javascript
-1
```

for descending order.

Example:

```javascript
db.students.createIndex({marks: 1})
```

Conceptually:

```text
40
45
52
60
72
85
95
```

Whereas:

```javascript
db.students.createIndex({marks: -1})
```

Conceptually:

```text
95
85
72
60
52
45
40
```

### Important

For a  **single-field index** , MongoDB can generally use the index for either direction of sorting, so `1` does not simply mean "this index can only support ascending queries."

---

# 5. Check Existing Indexes

Use:

```javascript
db.students.getIndexes()
```

Example output:

```javascript
[
    {
        v: 2,
        key: { _id: 1 },
        name: "_id_"
    },
    {
        v: 2,
        key: { age: 1 },
        name: "age_1"
    }
]
```

Notice that MongoDB automatically creates an index on:

```text
_id
```

---

# 6. The `_id` Index

Every MongoDB document normally has a unique `_id`.

Example:

```javascript
{
    _id: 101,
    name: "Rahul",
    age: 21
}
```

MongoDB automatically creates an index:

```javascript
{ _id: 1 }
```

This helps MongoDB efficiently locate documents using `_id`.

For example:

```javascript
db.students.find({_id: 101})
```

---

# 7. Single Field Index

A **single-field index** indexes one field.

Example:

```javascript
db.students.createIndex({age: 1})
```

Now queries involving `age` can potentially use this index.

```javascript
db.students.find({age: 20})
```

Range query:

```javascript
db.students.find({
    age: {$gt: 20}
})
```

Sorting:

```javascript
db.students.find().sort({age: 1})
```

---

# 8. Compound Index

A **compound index** contains multiple fields.

Example:

```javascript
db.students.createIndex({
    branch: 1,
    age: 1
})
```

This creates an index based on:

```text
branch + age
```

Conceptually:

```text
CSE      18
CSE      19
CSE      20
CSE      21
CSE      22

ECE      18
ECE      19
ECE      20
...
```

### Example query

```javascript
db.students.find({
    branch: "CSE",
    age: 20
})
```

This can make good use of the compound index.

---

# 9. The Important Rule: Prefix Rule

This is one of the  **most important concepts for students** .

Suppose we create:

```javascript
db.students.createIndex({
    branch: 1,
    age: 1,
    marks: 1
})
```

The index has the order:

```text
branch → age → marks
```

The **prefixes** are:

```text
branch
branch + age
branch + age + marks
```

Therefore, queries involving:

```javascript
{branch: "CSE"}
```

can use the index.

And:

```javascript
{
    branch: "CSE",
    age: 20
}
```

can use the index.

And:

```javascript
{
    branch: "CSE",
    age: 20,
    marks: {$gt: 70}
}
```

can use the index.

But a query only on:

```javascript
{age: 20}
```

does not have the `branch` prefix.

So it cannot use the compound index as effectively.

### Remember:

> **The order of fields in a compound index matters.**

---

# 10. Unique Index

A **unique index** prevents duplicate values.

Example:

```javascript
db.students.createIndex(
    {email: 1},
    {unique: true}
)
```

Now two students cannot have the same email.

First:

```javascript
{
    name: "Rahul",
    email: "rahul@gmail.com"
}
```

Allowed.

Second:

```javascript
{
    name: "Amit",
    email: "rahul@gmail.com"
}
```

MongoDB rejects it because the email already exists.

### Real-world uses

Unique indexes are useful for:

* Email IDs
* Employee IDs
* Student IDs
* Aadhaar-like application identifiers
* Usernames
* Registration numbers

---

# 11. Multikey Index

MongoDB can index  **array fields** .

Example:

```javascript
{
    name: "Rahul",
    skills: ["Java", "MongoDB", "Python"]
}
```

Create:

```javascript
db.students.createIndex({
    skills: 1
})
```

MongoDB creates a **multikey index** because `skills` is an array.

Now:

```javascript
db.students.find({
    skills: "MongoDB"
})
```

can use the index.

This is extremely useful when documents contain arrays.

---

# 12. Text Index

A **text index** is used for text-searching within string fields.

Example:

```javascript
{
    title: "Introduction to MongoDB",
    description: "MongoDB is a NoSQL database."
}
```

Create:

```javascript
db.books.createIndex({
    title: "text",
    description: "text"
})
```

Now we can search:

```javascript
db.books.find({
    $text: {
        $search: "MongoDB"
    }
})
```

This searches indexed text fields.

---

# 13. Geospatial Index

Geospatial indexes are used for location-based queries.

Suppose we store:

```javascript
{
    name: "Restaurant A",
    location: {
        type: "Point",
        coordinates: [77.1025, 28.7041]
    }
}
```

Create:

```javascript
db.restaurants.createIndex({
    location: "2dsphere"
})
```

`2dsphere` is commonly used for geographic data on a spherical Earth model.

Applications include:

* Nearby restaurants
* Cab services
* Delivery applications
* Hotels near a location
* Nearby hospitals

---

# 14. Hashed Index

A hashed index uses a **hash value** of the indexed field.

Example:

```javascript
db.users.createIndex({
    userId: "hashed"
})
```

This is useful particularly in scenarios involving  **hashed-based distribution** , including certain sharding designs.

Example:

```text
userId       Hash
1001         X73A
1002         K91B
1003         P42C
```

The actual ordering of the original values is not preserved in the way it is with a normal ascending index.

---

# 15. Sparse Index

A sparse index contains entries only for documents where the indexed field exists.

Example:

```javascript
db.students.createIndex(
    {phone: 1},
    {sparse: true}
)
```

Suppose:

```javascript
{ name: "Rahul", phone: "9999999999" }
{ name: "Amit" }
{ name: "Priya", phone: "8888888888" }
```

The sparse index contains entries for:

```text
Rahul
Priya
```

but not Amit because `phone` does not exist.

---

# 16. Partial Index

A **partial index** indexes only documents satisfying a specified condition.

Example:

```javascript
db.students.createIndex(
    {marks: 1},
    {
        partialFilterExpression: {
            marks: {$gte: 75}
        }
    }
)
```

This index focuses only on documents where:

```text
marks >= 75
```

For a large collection, this can reduce index size when only a subset of documents matters to the queries.

---

# 17. TTL Index

TTL means:

> **Time To Live**

A TTL index automatically removes documents after a specified amount of time.

Example:

```javascript
db.sessions.createIndex(
    {createdAt: 1},
    {expireAfterSeconds: 3600}
)
```

This means documents can automatically expire after approximately:

```text
3600 seconds
= 60 minutes
= 1 hour
```

Useful for:

* Temporary sessions
* OTP-related temporary records
* Logs
* Temporary tokens
* Cache-like data

---

# 18. Wildcard Index

A wildcard index can index unknown or dynamically varying fields.

Example document:

```javascript
{
    name: "Laptop",
    specifications: {
        RAM: "16GB",
        processor: "i7",
        storage: "1TB"
    }
}
```

If different documents have different fields inside `specifications`, a wildcard index can be useful.

Example:

```javascript
db.products.createIndex({
    "specifications.$**": 1
})
```

This indexes fields under:

```text
specifications
```

---

# 19. Index Types — Quick Summary

| Index Type   | Example                       | Main Purpose                           |
| ------------ | ----------------------------- | -------------------------------------- |
| `_id`      | `{_id: 1}`                  | Automatically created for document IDs |
| Single Field | `{age: 1}`                  | Search one field                       |
| Compound     | `{branch: 1, age: 1}`       | Search multiple fields                 |
| Unique       | `{email: 1}, {unique:true}` | Prevent duplicates                     |
| Multikey     | `{skills: 1}`               | Index arrays                           |
| Text         | `{title:"text"}`            | Text search                            |
| Geospatial   | `{location:"2dsphere"}`     | Location queries                       |
| Hashed       | `{userId:"hashed"}`         | Hash-based indexing/distribution       |
| Sparse       | `{phone:1}, {sparse:true}`  | Only documents containing field        |
| Partial      | `{marks:1}`+ condition      | Index selected documents               |
| TTL          | `{createdAt:1}`+ expiration | Automatically remove old documents     |
| Wildcard     | `{"specifications.$**":1}`  | Dynamic/nested fields                  |

---

# 20. Indexes and Query Performance

Consider:

```javascript
db.students.find({
    branch: "CSE",
    marks: {$gt: 80}
})
```

Without a suitable index:

```text
MongoDB
   ↓
Collection Scan
   ↓
Document 1 → check
Document 2 → check
Document 3 → check
...
Document 100000 → check
```

With:

```javascript
db.students.createIndex({
    branch: 1,
    marks: 1
})
```

MongoDB may use the index:

```text
Query
  ↓
Index
  ↓
branch = CSE
  ↓
marks > 80
  ↓
Matching documents
```

---

# 21. How Do We Know Whether MongoDB Uses an Index?

This is an important practical command:

```javascript
db.students.find({
    age: 20
}).explain("executionStats")
```

MongoDB provides information about query execution.

Look particularly at:

```text
COLLSCAN
```

and

```text
IXSCAN
```

### COLLSCAN

Means:

> Collection Scan

MongoDB is scanning documents in the collection.

### IXSCAN

Means:

> Index Scan

MongoDB is scanning an index.

For teaching, this is a great demonstration:

```javascript
db.students.find({age: 20}).explain("executionStats")
```

Create index:

```javascript
db.students.createIndex({age: 1})
```

Then run:

```javascript
db.students.find({age: 20}).explain("executionStats")
```

Compare the execution statistics.

---

# 22. Indexes Are Not Always Free

This is a very important point.

Indexes improve  **read performance** , but they also have costs.

### Additional storage

```text
Documents
   +
Indexes
   ↓
More disk space
```

### Insert/update overhead

When you insert:

```javascript
db.students.insertOne({
    name: "Rahul",
    age: 21
})
```

MongoDB may need to update relevant indexes as well.

Therefore:

> **Don't create an index on every field.**

Create indexes based on actual query patterns.

---

# 23. Delete an Index

First check:

```javascript
db.students.getIndexes()
```

Suppose you have:

```text
age_1
```

Remove it:

```javascript
db.students.dropIndex("age_1")
```

Or:

```javascript
db.students.dropIndex({age: 1})
```

---

# 24. Delete All User-Created Indexes

You can use:

```javascript
db.students.dropIndexes()
```

MongoDB keeps the required `_id` index.

---

# 25. A Practical Example for Students

Suppose we have:

```javascript
db.students.insertMany([
    {
        name: "Rahul",
        branch: "CSE",
        age: 21,
        marks: 85
    },
    {
        name: "Amit",
        branch: "CSE",
        age: 20,
        marks: 72
    },
    {
        name: "Priya",
        branch: "DS",
        age: 21,
        marks: 91
    }
])
```

### Step 1 — Search

```javascript
db.students.find({
    branch: "CSE"
})
```

### Step 2 — Create index

```javascript
db.students.createIndex({
    branch: 1
})
```

### Step 3 — Check index

```javascript
db.students.getIndexes()
```

### Step 4 — Analyze query

```javascript
db.students.find({
    branch: "CSE"
}).explain("executionStats")
```

### Step 5 — Compound index

```javascript
db.students.createIndex({
    branch: 1,
    marks: -1
})
```

Now a query such as:

```javascript
db.students.find({
    branch: "CSE"
}).sort({
    marks: -1
})
```

has an index whose field order aligns with the query pattern.

---

# 26. The Big Picture

You can teach indexing using this simple hierarchy:

```text
                    MONGODB INDEXING
                          |
          +---------------+---------------+
          |                               |
     Single Field                    Multiple Fields
          |                               |
      {age: 1}                    {branch: 1, age: 1}
                                          |
                                    Compound Index


Other Specialized Indexes
          |
    +-----+------+---------+---------+
    |     |      |         |         |
  Unique Multi  Text    Geo       TTL
          key           spatial
```

And the **four concepts students should remember first** are:

1. **Index = faster data access**
2. **`createIndex()` = create an index**
3. **Compound index field order matters**
4. **Indexes improve reads but consume storage and add write overhead**

### A very useful teaching sequence

Since you've already covered  **CRUD → operators → projection → sorting/limiting** , I'd teach the next MongoDB block as:

**Indexing → `createIndex()` → single-field index → compound index → unique index → multikey → text/geospatial → `explain()` → index management → query optimization.**

That sequence makes `explain("executionStats")` especially meaningful because students can actually  **see the difference between `COLLSCAN` and `IXSCAN`** .
