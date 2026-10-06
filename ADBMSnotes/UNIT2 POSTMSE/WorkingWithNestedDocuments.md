# Nested Documents in MongoDB

## What is a Nested Document?

A **Nested Document** (also called an  **Embedded Document** ) is a document stored inside another document as the value of a field.

### Example

```json
{
    "_id": 101,
    "name": "Aman",

    "address": {
        "city": "Ghaziabad",
        "state": "UP",
        "pincode": 201001
    }
}
```

Here, `address` is a nested document.

---

## Why Use Nested Documents?

Suppose every student has exactly one address and the address is meaningful only for that student.

Instead of creating a separate collection:

```text
Students
Addresses
```

we can embed the address inside the student document.

Benefits:

* Faster reads
* Fewer joins/lookups
* Simpler document structure
* Related data stored together

---

# Limitations / Restrictions

## 1. Maximum Document Size = 16 MB

MongoDB document size cannot exceed  **16 MB** .

Bad Example:

```text
Student
 └── Address
 └── Thousands of embedded records
 └── Large images
```

May exceed the limit.

---

## 2. Deep Nesting Makes Queries Complex

Allowed:

```json
{
  "address": {
      "city": "Ghaziabad"
  }
}
```

Not recommended:

```json
{
  "a":{
     "b":{
        "c":{
           "d":{
              "e":{
                 "f":"value"
              }
           }
        }
     }
  }
}
```

Very difficult to query and maintain.

---

## 3. Duplicate Embedded Data

If the same information is shared by many documents, embedding causes duplication.

Example:

```text
1000 Students
Same Department Information
```

Better to use referencing.

---

## 4. Entire Document Updated

MongoDB updates happen within the parent document.

Very large embedded documents may increase update cost.

---

# Common Operations on Nested Documents

Assume:

```json
{
    "_id":101,
    "name":"Aman",
    "address":{
        "city":"Ghaziabad",
        "state":"UP",
        "pincode":201001
    }
}
```

---

## Operations Table

| Operation                    | Purpose                                | Syntax                                                      | Expected Output         |
| ---------------------------- | -------------------------------------- | ----------------------------------------------------------- | ----------------------- |
| Insert Nested Document       | Create document with embedded document | `insertOne({...})`                                        | Document inserted       |
| Display Nested Document      | Show entire document                   | `find()`                                                  | Complete document       |
| Query Nested Field           | Search inside embedded document        | `find({"address.city":"Ghaziabad"})`                      | Matching documents      |
| Query Multiple Nested Fields | Match multiple conditions              | `find({"address.city":"Ghaziabad","address.state":"UP"})` | Matching documents      |
| Update Nested Field          | Modify one field                       | `$set`                                                    | Field updated           |
| Add New Nested Field         | Insert new property                    | `$set`                                                    | New field added         |
| Remove Nested Field          | Delete property                        | `$unset`                                                  | Field removed           |
| Replace Nested Document      | Replace whole object                   | `$set:{address:{...}}`                                    | Entire address replaced |
| Check Field Exists           | Verify nested field exists             | `$exists`                                                 | Matching documents      |
| Project Nested Field         | Display selected nested fields         | Projection                                                  | Limited output          |

---

# Frequently Used Operations

| Rank | Operation                      | Usage         |
| ---- | ------------------------------ | ------------- |
| 1    | Query Nested Field             | Very Common   |
| 2    | Update Nested Field            | Very Common   |
| 3    | Add Nested Field               | Common        |
| 4    | Remove Nested Field            | Common        |
| 5    | Projection                     | Common        |
| 6    | Check Existence                | Moderate      |
| 7    | Replace Entire Nested Document | Less Frequent |

For interviews and practical work, focus mainly on:

* Dot Notation
* Query Nested Fields
* Update Nested Fields
* Projection

---

# Step-by-Step Practical

---

## Step 1: Create Database

```javascript
use KIET
```

---

## Step 2: Create Collection

```javascript
db.students.insertOne({
    _id:101,
    name:"Aman",

    address:{
        city:"Ghaziabad",
        state:"UP",
        pincode:201001
    }
})
```

---

## Step 3: Display Data

```javascript
db.students.find().pretty()
```

Output:

```json
{
    "_id":101,
    "name":"Aman",
    "address":{
        "city":"Ghaziabad",
        "state":"UP",
        "pincode":201001
    }
}
```

---

# Operation 1: Query Nested Field

Find students from Ghaziabad.

```javascript
db.students.find({
    "address.city":"Ghaziabad"
})
```

Expected Output:

```json
Student document returned.
```

---

# Operation 2: Query Multiple Nested Fields

```javascript
db.students.find({
    "address.city":"Ghaziabad",
    "address.state":"UP"
})
```

Expected Output:

```json
Student document returned.
```

---

# Operation 3: Update Nested Field

Change city.

```javascript
db.students.updateOne(
    {_id:101},
    {
        $set:{
            "address.city":"Noida"
        }
    }
)
```

Verify:

```javascript
db.students.findOne({_id:101})
```

Output:

```json
{
    "address":{
        "city":"Noida",
        "state":"UP",
        "pincode":201001
    }
}
```

---

# Operation 4: Add New Nested Field

Add country.

```javascript
db.students.updateOne(
    {_id:101},
    {
        $set:{
            "address.country":"India"
        }
    }
)
```

Verify:

```javascript
db.students.findOne({_id:101})
```

Output:

```json
{
    "address":{
        "city":"Noida",
        "state":"UP",
        "pincode":201001,
        "country":"India"
    }
}
```

---

# Operation 5: Remove Nested Field

Remove pincode.

```javascript
db.students.updateOne(
    {_id:101},
    {
        $unset:{
            "address.pincode":""
        }
    }
)
```

Verify:

```javascript
db.students.findOne({_id:101})
```

Output:

```json
{
    "address":{
        "city":"Noida",
        "state":"UP",
        "country":"India"
    }
}
```

---

# Operation 6: Check Whether Nested Field Exists

```javascript
db.students.find({
    "address.country":{
        $exists:true
    }
})
```

Expected Output:

```json
Returns document because country exists.
```

---

# Operation 7: Projection of Nested Field

Display only city.

```javascript
db.students.find(
    {},
    {
        "address.city":1,
        _id:0
    }
)
```

Output:

```json
{
    "address":{
        "city":"Noida"
    }
}
```

---

# Operation 8: Replace Entire Nested Document

```javascript
db.students.updateOne(
    {_id:101},
    {
        $set:{
            address:{
                city:"Delhi",
                state:"Delhi",
                pincode:110001
            }
        }
    }
)
```

Verify:

```javascript
db.students.findOne({_id:101})
```

Output:

```json
{
    "address":{
        "city":"Delhi",
        "state":"Delhi",
        "pincode":110001
    }
}
```

Notice that the previous fields (`country`) disappeared because the whole nested document was replaced.

---

# Important Exam Point

### Dot Notation

MongoDB accesses nested document fields using  **dot notation** .

Example:

```javascript
"address.city"
```

Meaning:

```text
address
   └── city
```

Examples:

```javascript
db.students.find({"address.city":"Delhi"})
```

```javascript
db.students.updateOne(
 {},
 {$set:{"address.city":"Noida"}}
)
```

This is the **most important concept** when working with nested documents in MongoDB.
