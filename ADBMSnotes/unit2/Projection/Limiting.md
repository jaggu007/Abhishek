# Sorting and Limiting Results in MongoDB

Suppose we have a `students` collection:

```javascript
db.students.find()
```

Example documents:

```javascript
{
    name: "Rahul",
    branch: "CSE",
    age: 20,
    marks: 85
}

{
    name: "Priya",
    branch: "CSE",
    age: 21,
    marks: 92
}

{
    name: "Amit",
    branch: "DS",
    age: 19,
    marks: 78
}

{
    name: "Neha",
    branch: "CSE",
    age: 20,
    marks: 95
}
```

---

# 1. Sorting Results

MongoDB provides the **`sort()`** method.

### Syntax

```javascript
db.collection.find().sort({field: value})
```

The value determines the sorting direction:

|  Value | Meaning    |
| -----: | ---------- |
|  `1` | Ascending  |
| `-1` | Descending |

---

## Ascending Order

Suppose we want students sorted by marks from  **lowest to highest** :

```javascript
db.students.find().sort({marks: 1})
```

Result:

```text
Amit    78
Rahul   85
Priya   92
Neha    95
```

So:

```javascript
1
```

means  **ascending order** .

---

## Descending Order

Now suppose we want students with the  **highest marks first** :

```javascript
db.students.find().sort({marks: -1})
```

Result:

```text
Neha    95
Priya   92
Rahul   85
Amit    78
```

So:

```javascript
-1
```

means  **descending order** .

---

# 2. Sorting by Multiple Fields

This is very useful.

Suppose we want to sort students:

1. First by `branch` alphabetically
2. Then by `marks` from highest to lowest

```javascript
db.students.find().sort({
    branch: 1,
    marks: -1
})
```

MongoDB first sorts by:

```text
branch
```

and when two documents have the same branch, it uses:

```text
marks
```

to determine their order.

### Think of it like Excel sorting

You can think:

> **Primary sorting → Secondary sorting**

For example:

```text
CSE    95
CSE    92
CSE    85
DS     88
DS     78
```

---

# 3. Limiting Results

Sometimes we don't want all matching documents.

For example:

> Give me only the top 3 students.

MongoDB provides:

```javascript
limit()
```

### Syntax

```javascript
db.collection.find().limit(number)
```

Example:

```javascript
db.students.find().limit(3)
```

This returns only  **3 documents** .

---

# 4. Sorting + Limiting

This is where things become really useful.

### Problem

> Find the top 3 students based on marks.

We can write:

```javascript
db.students.find()
    .sort({marks: -1})
    .limit(3)
```

### What happens?

First:

```javascript
.sort({marks: -1})
```

puts the highest marks first.

Then:

```javascript
.limit(3)
```

takes only the first three documents.

So conceptually:

```text
All students
      ↓
Sort by marks ↓
      ↓
95
92
88
85
78
      ↓
Take first 3
      ↓
95
92
88
```

This is an extremely common MongoDB pattern.

---

# 5. `find()` + Projection + `sort()` + `limit()`

Now we can combine concepts you've already learned.

Suppose the requirement is:

> Display the top 3 students with only their name, branch and marks.

```javascript
db.students.find(
    {},
    {
        name: 1,
        branch: 1,
        marks: 1,
        _id: 0
    }
)
.sort({marks: -1})
.limit(3)
```

Output:

```text
{
    name: "Neha",
    branch: "CSE",
    marks: 95
}

{
    name: "Priya",
    branch: "CSE",
    marks: 92
}

{
    name: "Rahul",
    branch: "CSE",
    marks: 85
}
```

Notice how several MongoDB concepts are working together:

```text
find()
  ↓
Filtering
  ↓
Projection
  ↓
Sorting
  ↓
Limiting
```

---

# 6. Using a Condition + Sorting + Limit

Suppose:

> Find the top 3 CSE students.

```javascript
db.students.find(
    {branch: "CSE"}
)
.sort({marks: -1})
.limit(3)
```

Here:

```javascript
{branch: "CSE"}
```

filters the documents.

Then:

```javascript
.sort({marks: -1})
```

sorts them from highest to lowest.

Finally:

```javascript
.limit(3)
```

selects only three.

---

# 7. `skip()` — Useful with Limit

There is another method that goes nicely with `limit()`:

```javascript
skip()
```

It tells MongoDB to  **skip a certain number of documents** .

Example:

```javascript
db.students.find()
    .sort({marks: -1})
    .skip(3)
    .limit(3)
```

Suppose sorted results are:

```text
1. Neha     95
2. Priya    92
3. Rahul    90
4. Amit     88
5. Rohit    85
6. Ankit    80
```

After:

```javascript
.skip(3)
```

MongoDB skips:

```text
Neha
Priya
Rahul
```

Then:

```javascript
.limit(3)
```

returns:

```text
Amit
Rohit
Ankit
```

---

# 8. Pagination

This combination is heavily used in real applications.

Suppose a website displays  **10 students per page** .

### Page 1

```javascript
db.students.find()
    .sort({name: 1})
    .skip(0)
    .limit(10)
```

### Page 2

```javascript
db.students.find()
    .sort({name: 1})
    .skip(10)
    .limit(10)
```

### Page 3

```javascript
db.students.find()
    .sort({name: 1})
    .skip(20)
    .limit(10)
```

The general formula is:

```text
skip = (pageNumber - 1) × pageSize
```

For example:

```text
Page 1 → (1-1) × 10 = 0
Page 2 → (2-1) × 10 = 10
Page 3 → (3-1) × 10 = 20
Page 4 → (4-1) × 10 = 30
```

---

# 9. Important Point: Order Matters

For teaching purposes, remember this pattern:

```javascript
find()
    .sort()
    .skip()
    .limit()
```

For example:

```javascript
db.students.find({branch: "CSE"})
    .sort({marks: -1})
    .skip(5)
    .limit(5)
```

Conceptually:

```text
Find CSE students
       ↓
Sort by marks
       ↓
Skip first 5
       ↓
Take next 5
```

This is useful for  **ranking and pagination** .

---

# 10. Practical Examples

### Example 1 — Highest marks

```javascript
db.students.find().sort({marks: -1})
```

### Example 2 — Lowest marks

```javascript
db.students.find().sort({marks: 1})
```

### Example 3 — Top 5 students

```javascript
db.students.find()
    .sort({marks: -1})
    .limit(5)
```

### Example 4 — Bottom 5 students

```javascript
db.students.find()
    .sort({marks: 1})
    .limit(5)
```

### Example 5 — Top 3 CSE students

```javascript
db.students.find({branch: "CSE"})
    .sort({marks: -1})
    .limit(3)
```

### Example 6 — Students from oldest to youngest

```javascript
db.students.find()
    .sort({age: -1})
```

### Example 7 — Students from youngest to oldest

```javascript
db.students.find()
    .sort({age: 1})
```

---

## ⭐ The Big Picture

You can remember these four methods as:

```text
find()   → Which documents?
sort()   → In what order?
skip()   → Ignore how many?
limit()  → Return how many?
```

For example:

```javascript
db.students.find(
    {branch: "CSE"},
    {name: 1, marks: 1, _id: 0}
)
.sort({marks: -1})
.skip(5)
.limit(5)
```

Read this almost like English:

> **Find CSE students, show only name and marks, arrange them from highest to lowest, skip the first 5, and show the next 5.**
