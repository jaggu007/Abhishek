### Arrays in MongoDB

An **array** in MongoDB is a field that stores  **multiple values within a single document** . The values can be of the same type (e.g., strings, numbers) or different types.

**Example:**

```json
{
    "_id": 101,
    "name": "Aman",
    "skills": ["Java", "Python", "MongoDB"]
}
```

Here, `skills` is an array containing multiple values: `"Java"`, `"Python"`, and `"MongoDB"`.

### Can an array have mixed types of values?

**Yes.** MongoDB is schema-flexible, so an array can store values of different data types.

**Example:**

```json
{
    "_id": 101,
    "data": [
        "Java",
        100,
        true,
        85.5,
        {
            "city": "Ghaziabad"
        }
    ]
}
```

Here the array contains:

* String → `"Java"`
* Integer → `100`
* Boolean → `true`
* Double → `85.5`
* Embedded Document → `{ "city": "Ghaziabad" }`

MongoDB allows this.

---

### Can we restrict an array to contain only similar types?

**Yes, but not automatically.** MongoDB itself does not enforce this by default.

To restrict an array to a specific type, you can use:

#### A. Application-Level Validation

Your Java/Python/Node.js code checks values before inserting them.

Example:

```json
"skills": ["Java", "Python", "MongoDB"]
```

Only strings are allowed by your application logic.

---

#### B. Schema Validation (Recommended)

MongoDB supports  **JSON Schema Validation** .

Example:

```javascript
db.createCollection("students", {
   validator: {
      $jsonSchema: {
         bsonType: "object",
         properties: {
            skills: {
               bsonType: "array",
               items: {
                  bsonType: "string"
               }
            }
         }
      }
   }
})
```

Now MongoDB will accept:

```json
{
    "skills": ["Java", "Python"]
}
```

but reject:

```json
{
    "skills": ["Java", 100, true]
}
```

## Common Array Operations in MongoDB

| Operation                                                                                                                                                                             | One-Line Explanation                                     | Syntax                                    | Expected Output                              |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------- | ----------------------------------------- | -------------------------------------------- |
| Add Element (`$push`)                 | Adds a new element at the end of an array.               | `db.students.updateOne({_id:101},{$push:{skills:"React"}})`                    | `"React"`added to `skills`array.                     |                                           |                                              |
| Add Multiple Elements (`$each`)       | Adds multiple elements in a single operation.            | `db.students.updateOne({_id:101},{$push:{skills:{$each:["React","NodeJS"]}}})` | Both values added to array.                              |                                           |                                              |
| Add Unique Element (`$addToSet`)      | Adds element only if it does not already exist.          | `db.students.updateOne({_id:101},{$addToSet:{skills:"Java"}})`                 | No duplicate `"Java"`inserted.                         |                                           |                                              |
| Remove Element (`$pull`)              | Removes all matching elements from an array.             | `db.students.updateOne({_id:101},{$pull:{skills:"Python"}})`                   | `"Python"`removed from array.                          |                                           |                                              |
| Remove Multiple Elements (`$pullAll`) | Removes multiple specified values.                       | `db.students.updateOne({_id:101},{$pullAll:{skills:["Java","Python"]}})`       | Both values removed.                                     |                                           |                                              |
| Remove First Element (`$pop`)         | Removes the first element of an array.                   | `db.students.updateOne({_id:101},{$pop:{skills:-1}})`                          | First element removed.                                   |                                           |                                              |
| Remove Last Element (`$pop`)          | Removes the last element of an array.                    | `db.students.updateOne({_id:101},{$pop:{skills:1}})`                           | Last element removed.                                    |                                           |                                              |
| Check Element Exists                                                                                                                                                                  | Finds documents containing a specific value in an array. | `db.students.find({skills:"Java"})`     | Returns documents having `"Java"`in array. |
| Match All Elements (`$all`)           | Finds documents containing all specified values.         | `db.students.find({skills:{$all:["Java","MongoDB"]}})`                         | Returns documents containing both values.                |                                           |                                              |
| Array Size (`$size`)                  | Finds arrays having an exact number of elements.         | `db.students.find({skills:{$size:3}})`                                         | Returns arrays with exactly 3 items.                     |                                           |                                              |
| Find by Index                                                                                                                                                                         | Access a specific array position.                        | `db.students.find({"skills.0":"Java"})` | Returns documents whose first skill is Java. |
| Update by Index (`$set`)              | Replaces value at a specific index.                      | `db.students.updateOne({_id:101},{$set:{"skills.1":"React"}})`                 | Element at index 1 changed to React.                     |                                           |                                              |
| Check Array Exists (`$exists`)        | Checks whether an array field exists.                    | `db.students.find({skills:{$exists:true}})`                                    | Returns documents containing `skills`.                 |                                           |                                              |
| Slice Array (`$slice`)                | Returns only a portion of an array.                      | `db.students.find({},{skills:{$slice:2}})`                                     | Displays first 2 elements.                               |                                           |                                              |
| Last N Elements (`$slice`)            | Returns last N elements of an array.                     | `db.students.find({},{skills:{$slice:-2}})`                                    | Displays last 2 elements.                                |                                           |                                              |
| Element Match (`$elemMatch`)          | Matches array elements satisfying multiple conditions.   | `db.students.find({marks:{$elemMatch:{subject:"Java",score:{$gt:90}}}})`       | Returns matching documents.                              |                                           |                                              |
| Array Contains Any (`$in`)            | Finds arrays containing at least one specified value.    | `db.students.find({skills:{$in:["Java","Python"]}})`                           | Returns documents containing either value.               |                                           |                                              |
| Array Contains None (`$nin`)          | Finds arrays not containing specified values.            | `db.students.find({skills:{$nin:["Java"]}})`                                   | Returns documents without Java.                          |                                           |                                              |

---

### Sample Document Used

```json
{
    "_id": 101,
    "name": "Aman",
    "skills": ["Java", "Python", "MongoDB"],
    "marks": [
        {"subject":"Java","score":95},
        {"subject":"DBMS","score":88}
    ]
}
```

### Most Frequently Used Array Operations in Practice

| Rank | Operation      | Purpose                       |
| ---- | -------------- | ----------------------------- |
| 1    | `$push`      | Add element                   |
| 2    | `$pull`      | Remove element                |
| 3    | `$addToSet`  | Prevent duplicates            |
| 4    | `$all`       | Match multiple values         |
| 5    | `$size`      | Count elements                |
| 6    | `$elemMatch` | Query array of documents      |
| 7    | `$slice`     | Retrieve partial array        |
| 8    | `$set`       | Update specific index         |
| 9    | `$pop`       | Remove first/last element     |
| 10   | `$in`        | Search for any matching value |

### MongoDB, focus first on  **`$push`, `$pull`, `$addToSet`, `$all`, `$size`, and `$elemMatch`** , as these cover about 80% of real-world array operations.



Below is a  **step-by-step MongoDB Shell (mongosh) script** . Run each section one by one and observe the output.

---

# 0. Create Database and Collection

```javascript
use KIET
```

```javascript
db.students.insertOne({
    _id: 101,
    name: "Aman",
    skills: ["Java", "Python", "MongoDB"],
    marks: [
        { subject: "Java", score: 95 },
        { subject: "DBMS", score: 88 }
    ]
})
```

Verify:

```javascript
db.students.find().pretty()
```

---

# 1. Add One Element (`$push`)

### Before

```javascript
db.students.findOne({_id:101})
```

### Operation

```javascript
db.students.updateOne(
    {_id:101},
    {$push:{skills:"React"}}
)
```

### After

```javascript
db.students.findOne({_id:101})
```

Expected:

```json
"skills": ["Java","Python","MongoDB","React"]
```

---

# 2. Add Multiple Elements (`$each`)

### Operation

```javascript
db.students.updateOne(
    {_id:101},
    {
        $push:{
            skills:{
                $each:["NodeJS","Angular"]
            }
        }
    }
)
```

### Verify

```javascript
db.students.findOne({_id:101})
```

Expected:

```json
["Java","Python","MongoDB","React","NodeJS","Angular"]
```

---

# 3. Add Unique Element (`$addToSet`)

### Existing Value

```javascript
db.students.updateOne(
    {_id:101},
    {$addToSet:{skills:"Java"}}
)
```

### Verify

```javascript
db.students.findOne({_id:101})
```

Expected:

```json
No duplicate Java added.
```

---

### New Value

```javascript
db.students.updateOne(
    {_id:101},
    {$addToSet:{skills:"SpringBoot"}}
)
```

Expected:

```json
SpringBoot added.
```

---

# 4. Remove One Element (`$pull`)

### Operation

```javascript
db.students.updateOne(
    {_id:101},
    {$pull:{skills:"Python"}}
)
```

### Verify

```javascript
db.students.findOne({_id:101})
```

Expected:

```json
Python removed.
```

---

# 5. Remove Multiple Elements (`$pullAll`)

### Operation

```javascript
db.students.updateOne(
    {_id:101},
    {
        $pullAll:{
            skills:["React","Angular"]
        }
    }
)
```

### Verify

```javascript
db.students.findOne({_id:101})
```

Expected:

```json
React and Angular removed.
```

---

# 6. Remove First Element (`$pop : -1`)

### Before

```javascript
db.students.findOne({_id:101})
```

### Operation

```javascript
db.students.updateOne(
    {_id:101},
    {$pop:{skills:-1}}
)
```

### Verify

```javascript
db.students.findOne({_id:101})
```

Expected:

```json
First element removed.
```

---

# 7. Remove Last Element (`$pop : 1`)

### Operation

```javascript
db.students.updateOne(
    {_id:101},
    {$pop:{skills:1}}
)
```

### Verify

```javascript
db.students.findOne({_id:101})
```

Expected:

```json
Last element removed.
```

---

# 8. Search Element in Array

### Operation

```javascript
db.students.find({
    skills:"MongoDB"
})
```

Expected:

```json
Document returned because MongoDB exists in array.
```

---

# 9. Match Multiple Values (`$all`)

### Operation

```javascript
db.students.find({
    skills:{
        $all:["Java","MongoDB"]
    }
})
```

Expected:

```json
Returns document only if both values exist.
```

---

# 10. Find Array Size (`$size`)

### Operation

```javascript
db.students.find({
    skills:{
        $size:3
    }
})
```

Expected:

```json
Returns document if array contains exactly 3 elements.
```

---

# 11. Access Array Element by Index

### Operation

```javascript
db.students.find({
    "skills.0":"Java"
})
```

Expected:

```json
Returns document if first element is Java.
```

---

# 12. Update Element by Index

### Before

```javascript
db.students.findOne({_id:101})
```

### Operation

```javascript
db.students.updateOne(
    {_id:101},
    {
        $set:{
            "skills.1":"C++"
        }
    }
)
```

### Verify

```javascript
db.students.findOne({_id:101})
```

Expected:

```json
Second element changed to C++.
```

---

# 13. Check Array Field Exists

### Operation

```javascript
db.students.find({
    skills:{
        $exists:true
    }
})
```

Expected:

```json
Returns documents containing skills field.
```

---

# 14. Display First 2 Elements (`$slice`)

### Operation

```javascript
db.students.find(
    {},
    {
        skills:{
            $slice:2
        }
    }
)
```

Expected:

```json
Only first 2 elements displayed.
```

---

# 15. Display Last 2 Elements (`$slice`)

### Operation

```javascript
db.students.find(
    {},
    {
        skills:{
            $slice:-2
        }
    }
)
```

Expected:

```json
Only last 2 elements displayed.
```

---

# 16. Array of Documents Query (`$elemMatch`)

### Operation

```javascript
db.students.find({
    marks:{
        $elemMatch:{
            subject:"Java",
            score:{$gt:90}
        }
    }
})
```

Expected:

```json
Returns document because Java score is 95.
```

---

# 17. Match Any Value (`$in`)

### Operation

```javascript
db.students.find({
    skills:{
        $in:["Python","MongoDB"]
    }
})
```

Expected:

```json
Returns document if any one value exists.
```

---

# 18. Match None of the Values (`$nin`)

### Operation

```javascript
db.students.find({
    skills:{
        $nin:["PHP"]
    }
})
```

Expected:

```json
Returns document because PHP does not exist.
```

---

# Quick Lab Exercise for Students

```javascript
use KIET

db.students.drop()

db.students.insertOne({
    _id:101,
    name:"Aman",
    skills:["Java","Python","MongoDB"],
    marks:[
        {subject:"Java",score:95},
        {subject:"DBMS",score:88}
    ]
})
```

Now  perform:

1. Add React
2. Add NodeJS and Angular
3. Remove Python
4. Search MongoDB skill
5. Find students having Java and MongoDB
6. Change Python to C++
7. Display first two skills
8. Display last two skills
9. Find students scoring above 90 in Java
10. Prevent duplicate Java insertion using `$addToSet`

These 10 exercises cover the most important array operations used in real MongoDB applications.
