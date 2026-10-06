Here's a **sample CSV dataset with 100 student records** suitable for MongoDB practice. It includes:

* `student_id`
* `name`
* `age`
* `gender`
* `branch`
* `semester`
* `cgpa`
* `city`

You can save this as **students.csv** and import it into MongoDB.

```csv
student_id,name,age,gender,branch,semester,cgpa,city
S001,Aarav Sharma,18,M,CSE,1,8.1,Delhi
S002,Ananya Gupta,19,F,IT,2,8.5,Noida
S003,Rohan Verma,20,M,CSE,3,7.8,Ghaziabad
S004,Priya Singh,21,F,DS,4,9.1,Lucknow
S005,Karan Mehta,18,M,AIML,1,8.7,Kanpur
S006,Neha Jain,19,F,CSE,2,8.2,Delhi
S007,Aditya Kumar,20,M,IT,3,7.5,Noida
S008,Sneha Agarwal,21,F,DS,4,8.9,Ghaziabad
S009,Vivek Mishra,22,M,AIML,5,7.9,Lucknow
S010,Shreya Saxena,18,F,CSE,1,9.3,Kanpur
S011,Rahul Sharma,19,M,IT,2,8.0,Delhi
S012,Kriti Gupta,20,F,CSE,3,8.4,Noida
S013,Manish Verma,21,M,DS,4,7.6,Ghaziabad
S014,Pooja Singh,22,F,AIML,5,8.8,Lucknow
S015,Arjun Mehta,18,M,CSE,1,8.9,Kanpur
S016,Ritika Jain,19,F,IT,2,7.7,Delhi
S017,Harsh Kumar,20,M,CSE,3,8.6,Noida
S018,Nidhi Agarwal,21,F,DS,4,9.0,Ghaziabad
S019,Ayush Mishra,22,M,AIML,5,7.4,Lucknow
S020,Simran Saxena,18,F,CSE,1,8.3,Kanpur
S021,Aarav Sharma,19,M,IT,2,8.1,Delhi
S022,Ananya Gupta,20,F,CSE,3,8.5,Noida
S023,Rohan Verma,21,M,DS,4,7.8,Ghaziabad
S024,Priya Singh,22,F,AIML,5,9.1,Lucknow
S025,Karan Mehta,18,M,CSE,1,8.7,Kanpur
S026,Neha Jain,19,F,IT,2,8.2,Delhi
S027,Aditya Kumar,20,M,CSE,3,7.5,Noida
S028,Sneha Agarwal,21,F,DS,4,8.9,Ghaziabad
S029,Vivek Mishra,22,M,AIML,5,7.9,Lucknow
S030,Shreya Saxena,18,F,CSE,1,9.3,Kanpur
S031,Rahul Sharma,19,M,IT,2,8.0,Delhi
S032,Kriti Gupta,20,F,CSE,3,8.4,Noida
S033,Manish Verma,21,M,DS,4,7.6,Ghaziabad
S034,Pooja Singh,22,F,AIML,5,8.8,Lucknow
S035,Arjun Mehta,18,M,CSE,1,8.9,Kanpur
S036,Ritika Jain,19,F,IT,2,7.7,Delhi
S037,Harsh Kumar,20,M,CSE,3,8.6,Noida
S038,Nidhi Agarwal,21,F,DS,4,9.0,Ghaziabad
S039,Ayush Mishra,22,M,AIML,5,7.4,Lucknow
S040,Simran Saxena,18,F,CSE,1,8.3,Kanpur
S041,Aarav Sharma,19,M,IT,2,8.1,Delhi
S042,Ananya Gupta,20,F,CSE,3,8.5,Noida
S043,Rohan Verma,21,M,DS,4,7.8,Ghaziabad
S044,Priya Singh,22,F,AIML,5,9.1,Lucknow
S045,Karan Mehta,18,M,CSE,1,8.7,Kanpur
S046,Neha Jain,19,F,IT,2,8.2,Delhi
S047,Aditya Kumar,20,M,CSE,3,7.5,Noida
S048,Sneha Agarwal,21,F,DS,4,8.9,Ghaziabad
S049,Vivek Mishra,22,M,AIML,5,7.9,Lucknow
S050,Shreya Saxena,18,F,CSE,1,9.3,Kanpur
S051,Rahul Sharma,19,M,IT,2,8.0,Delhi
S052,Kriti Gupta,20,F,CSE,3,8.4,Noida
S053,Manish Verma,21,M,DS,4,7.6,Ghaziabad
S054,Pooja Singh,22,F,AIML,5,8.8,Lucknow
S055,Arjun Mehta,18,M,CSE,1,8.9,Kanpur
S056,Ritika Jain,19,F,IT,2,7.7,Delhi
S057,Harsh Kumar,20,M,CSE,3,8.6,Noida
S058,Nidhi Agarwal,21,F,DS,4,9.0,Ghaziabad
S059,Ayush Mishra,22,M,AIML,5,7.4,Lucknow
S060,Simran Saxena,18,F,CSE,1,8.3,Kanpur
S061,Aarav Sharma,19,M,IT,2,8.1,Delhi
S062,Ananya Gupta,20,F,CSE,3,8.5,Noida
S063,Rohan Verma,21,M,DS,4,7.8,Ghaziabad
S064,Priya Singh,22,F,AIML,5,9.1,Lucknow
S065,Karan Mehta,18,M,CSE,1,8.7,Kanpur
S066,Neha Jain,19,F,IT,2,8.2,Delhi
S067,Aditya Kumar,20,M,CSE,3,7.5,Noida
S068,Sneha Agarwal,21,F,DS,4,8.9,Ghaziabad
S069,Vivek Mishra,22,M,AIML,5,7.9,Lucknow
S070,Shreya Saxena,18,F,CSE,1,9.3,Kanpur
S071,Rahul Sharma,19,M,IT,2,8.0,Delhi
S072,Kriti Gupta,20,F,CSE,3,8.4,Noida
S073,Manish Verma,21,M,DS,4,7.6,Ghaziabad
S074,Pooja Singh,22,F,AIML,5,8.8,Lucknow
S075,Arjun Mehta,18,M,CSE,1,8.9,Kanpur
S076,Ritika Jain,19,F,IT,2,7.7,Delhi
S077,Harsh Kumar,20,M,CSE,3,8.6,Noida
S078,Nidhi Agarwal,21,F,DS,4,9.0,Ghaziabad
S079,Ayush Mishra,22,M,AIML,5,7.4,Lucknow
S080,Simran Saxena,18,F,CSE,1,8.3,Kanpur
S081,Aarav Sharma,19,M,IT,2,8.1,Delhi
S082,Ananya Gupta,20,F,CSE,3,8.5,Noida
S083,Rohan Verma,21,M,DS,4,7.8,Ghaziabad
S084,Priya Singh,22,F,AIML,5,9.1,Lucknow
S085,Karan Mehta,18,M,CSE,1,8.7,Kanpur
S086,Neha Jain,19,F,IT,2,8.2,Delhi
S087,Aditya Kumar,20,M,CSE,3,7.5,Noida
S088,Sneha Agarwal,21,F,DS,4,8.9,Ghaziabad
S089,Vivek Mishra,22,M,AIML,5,7.9,Lucknow
S090,Shreya Saxena,18,F,CSE,1,9.3,Kanpur
S091,Rahul Sharma,19,M,IT,2,8.0,Delhi
S092,Kriti Gupta,20,F,CSE,3,8.4,Noida
S093,Manish Verma,21,M,DS,4,7.6,Ghaziabad
S094,Pooja Singh,22,F,AIML,5,8.8,Lucknow
S095,Arjun Mehta,18,M,CSE,1,8.9,Kanpur
S096,Ritika Jain,19,F,IT,2,7.7,Delhi
S097,Harsh Kumar,20,M,CSE,3,8.6,Noida
S098,Nidhi Agarwal,21,F,DS,4,9.0,Ghaziabad
S099,Ayush Mishra,22,M,AIML,5,7.4,Lucknow
S100,Simran Saxena,18,F,CSE,1,8.3,Kanpur
```

### Import into MongoDB

```bash
mongoimport \
--db KIET \
--collection students \
--type csv \
--headerline \
--file students.csv
```

This dataset is useful for practicing:

* CRUD operations
* Query operators (`$gt`, `$lt`, `$in`, `$and`, `$or`)
* Aggregation (`$group`, `$avg`, `$sum`)
* Sorting and limiting
* Indexing

=========================================================

### Here are **practice queries for each MongoDB Comparison Operator** using the `students` collection from your dataset.

## 1. `$eq` (Equal To)

**Find all students from Delhi**

```javascript
db.students.find({
    city: { $eq: "Delhi" }
})
```

**Find students with CGPA exactly 8.5**

```javascript
db.students.find({
    cgpa: { $eq: 8.5 }
})
```

---

## 2. `$ne` (Not Equal To)

**Find students who are not from Delhi**

```javascript
db.students.find({
    city: { $ne: "Delhi" }
})
```

**Find students whose branch is not CSE**

```javascript
db.students.find({
    branch: { $ne: "CSE" }
})
```

---

## 3. `$gt` (Greater Than)

**Find students older than 20**

```javascript
db.students.find({
    age: { $gt: 20 }
})
```

**Find students with CGPA greater than 8.5**

```javascript
db.students.find({
    cgpa: { $gt: 8.5 }
})
```

---

## 4. `$gte` (Greater Than or Equal To)

**Find students aged 21 or above**

```javascript
db.students.find({
    age: { $gte: 21 }
})
```

**Find students with CGPA 9.0 or higher**

```javascript
db.students.find({
    cgpa: { $gte: 9.0 }
})
```

---

## 5. `$lt` (Less Than)

**Find students younger than 20**

```javascript
db.students.find({
    age: { $lt: 20 }
})
```

**Find students with CGPA below 8.0**

```javascript
db.students.find({
    cgpa: { $lt: 8.0 }
})
```

---

## 6. `$lte` (Less Than or Equal To)

**Find students aged 19 or below**

```javascript
db.students.find({
    age: { $lte: 19 }
})
```

**Find students with CGPA 8.0 or below**

```javascript
db.students.find({
    cgpa: { $lte: 8.0 }
})
```

---

## 7. `$in` (Matches Any Value in List)

**Find students from Delhi or Noida**

```javascript
db.students.find({
    city: {
        $in: ["Delhi", "Noida"]
    }
})
```

**Find students from CSE or AIML branch**

```javascript
db.students.find({
    branch: {
        $in: ["CSE", "AIML"]
    }
})
```

---

## 8. `$nin` (Not In List)

**Find students who are not from Delhi or Noida**

```javascript
db.students.find({
    city: {
        $nin: ["Delhi", "Noida"]
    }
})
```

**Find students who are not in CSE or IT**

```javascript
db.students.find({
    branch: {
        $nin: ["CSE", "IT"]
    }
})
```

---

## Combined Comparison Operators

### Find students aged between 19 and 21

```javascript
db.students.find({
    age: {
        $gte: 19,
        $lte: 21
    }
})
```

### Find students with CGPA between 8.0 and 9.0

```javascript
db.students.find({
    cgpa: {
        $gte: 8.0,
        $lte: 9.0
    }
})
```

### Find CSE students with CGPA above 8.5

```javascript
db.students.find({
    branch: "CSE",
    cgpa: { $gt: 8.5 }
})
```

### Find AIML students aged less than 22

```javascript
db.students.find({
    branch: "AIML",
    age: { $lt: 22 }
})
```

### Find students from Delhi with CGPA at least 8.0

```javascript
db.students.find({
    city: "Delhi",
    cgpa: { $gte: 8.0 }
})
```

=====================================================

### Logical operators are where MongoDB queries start becoming really powerful because you can combine multiple conditions.

# 1. `$and` Operator

Returns documents that satisfy  **all conditions** .

### Example 1

Find students from CSE **and** with CGPA greater than 8.5.

```javascript
db.students.find({
    $and: [
        { branch: "CSE" },
        { cgpa: { $gt: 8.5 } }
    ]
})
```

### Example 2

Find students from Delhi and age greater than 18.

```javascript
db.students.find({
    $and: [
        { city: "Delhi" },
        { age: { $gt: 18 } }
    ]
})
```

### Short Form (Most Common)

MongoDB automatically applies AND between fields.

```javascript
db.students.find({
    city: "Delhi",
    age: { $gt: 18 }
})
```

---

# 2. `$or` Operator

Returns documents that satisfy  **at least one condition** .

### Example 1

Find students from Delhi or Noida.

```javascript
db.students.find({
    $or: [
        { city: "Delhi" },
        { city: "Noida" }
    ]
})
```

### Example 2

Find students from CSE or AIML.

```javascript
db.students.find({
    $or: [
        { branch: "CSE" },
        { branch: "AIML" }
    ]
})
```

---

# 3. `$not` Operator

Negates a condition.

### Example 1

Find students whose age is NOT greater than 20.

```javascript
db.students.find({
    age: {
        $not: { $gt: 20 }
    }
})
```

Equivalent to:

```javascript
db.students.find({
    age: { $lte: 20 }
})
```

### Example 2

Find students whose CGPA is NOT below 8.

```javascript
db.students.find({
    cgpa: {
        $not: { $lt: 8.0 }
    }
})
```

---

# 4. `$nor` Operator

Returns documents that fail all specified conditions.

### Example 1

Find students who are neither from Delhi nor Noida.

```javascript
db.students.find({
    $nor: [
        { city: "Delhi" },
        { city: "Noida" }
    ]
})
```

### Example 2

Find students who are neither in CSE nor IT.

```javascript
db.students.find({
    $nor: [
        { branch: "CSE" },
        { branch: "IT" }
    ]
})
```

---

# Practice Questions

### Easy

1. Find students from Delhi and branch CSE.
2. Find students from Noida or Ghaziabad.
3. Find students whose age is not greater than 21.
4. Find students who are neither from Delhi nor Lucknow.
5. Find students from AIML and CGPA greater than 8.

---

### Moderate

6. Find students from CSE or IT whose CGPA is above 8.5.

```javascript
db.students.find({
    $and: [
        {
            $or: [
                { branch: "CSE" },
                { branch: "IT" }
            ]
        },
        { cgpa: { $gt: 8.5 } }
    ]
})
```

7. Find students from Delhi or Noida who are younger than 20.
8. Find students whose branch is not AIML and CGPA is above 8.
9. Find students who are neither from Kanpur nor Lucknow and have CGPA above 8.
10. Find students from DS or AIML whose age is at least 21.

---

# Complex Nested Query Example

**Find students who:**

* belong to CSE or IT
* have CGPA above 8.0
* are not from Lucknow

```javascript
db.students.find({
    $and: [
        {
            $or: [
                { branch: "CSE" },
                { branch: "IT" }
            ]
        },
        { cgpa: { $gt: 8.0 } },
        {
            city: {
                $not: { $eq: "Lucknow" }
            }
        }
    ]
})
```

## Quick Exam Tip

| Operator | Meaning                     |
| -------- | --------------------------- |
| `$and` | All conditions true         |
| `$or`  | Any one condition true      |
| `$not` | Negates a condition         |
| `$nor` | None of the conditions true |
