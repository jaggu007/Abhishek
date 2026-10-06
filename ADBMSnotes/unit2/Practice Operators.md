
## MongoDB Operators – 15 Scenario-Based Practice Questions

Assume the following collections are available: `students`, `employees`, `products`, `orders`, `courses`, and `patients`.

### 1. Student Scholarship Eligibility

A university wants to identify students eligible for a merit scholarship.

The `students` collection contains:

```javascript
{
  studentId: 101,
  name: "Rahul",
  branch: "CSE",
  cgpa: 8.7,
  attendance: 91,
  year: 2
}
```

**Task:** Find students whose **CGPA is greater than 8.0** and  **attendance is at least 85%** .

**Operators:** `$gt`, `$gte`, `$and`

---

### 2. High-Paid Employees

The HR department wants to identify employees who are either highly experienced or highly paid.

**Task:** Find employees who have  **more than 8 years of experience OR salary greater than ₹80,000** .

**Operators:** `$gt`, `$or`

---

### 3. Electronics Product Filter

An e-commerce company wants to find affordable electronic products.

**Task:** Find products where:

* Category is `"Electronics"`
* Price is between **₹10,000 and ₹50,000**

**Operators:** `$eq`, `$gte`, `$lte`, `$and`

---

### 4. Students from Selected Branches

The placement cell wants a list of students belonging to either  **CSE, AI, or Data Science** .

**Task:** Find students whose branch belongs to these three branches.

**Hint:** Use the `$in` operator.

---

### 5. Employees Not in Management

The HR department wants to identify employees who are  **not working in the HR or Finance departments** .

**Task:** Find all employees whose department is neither `"HR"` nor `"Finance"`.

**Operators:** `$nin`, `$not`

---

### 6. Missing Contact Information

The college database administrator wants to find student records where the  **email field is missing** .

**Task:** Find students who do not have an `email` field.

**Operator:** `$exists`

Then modify the requirement:

> Find students whose `phone` field exists.

---

### 7. Product Reviews Analysis

An e-commerce website wants to identify products that have received customer reviews.

Suppose a product document contains:

```javascript
{
  name: "Laptop",
  reviews: [
    {user: "Amit", rating: 5},
    {user: "Neha", rating: 4}
  ]
}
```

**Task:** Find products where the `reviews` field exists and contains at least one review.

**Operators:** `$exists`, array-related querying

---

### 8. Patients Requiring Attention

A hospital wants to identify patients who satisfy **any one** of the following conditions:

* Age is above 65
* Disease is `"Diabetes"`
* Blood pressure is above 140

**Task:** Write a query to retrieve such patients.

**Operators:** `$or`, `$gt`, `$eq`

---

### 9. Course Fee Filter

An online learning platform wants to recommend courses that are reasonably priced.

**Task:** Find courses where the fee is:

* Greater than ₹2,000
* Less than or equal to ₹8,000
* Category is either `"AI"` or `"Data Science"`

**Operators:** `$gt`, `$lte`, `$in`, `$and`

---

### 10. Salary Range Using `$not`

An organization wants to find employees whose salary is  **not greater than ₹1,00,000** .

**Task:** Write a query using the `$not` operator rather than directly using `$lte`.

**Operator:** `$not`

---

### 11. Student Name Pattern

The admission department wants to find students whose names  **start with the letter "A"** .

**Task:** Write a MongoDB query to find these students.

**Operator:** `$regex`

Then modify it to find students whose names  **end with `"sh"`** .

---

### 12. Searching Project Titles

A project coordinator wants to find all projects whose title contains the word `"AI"` anywhere in the title.

For example:

```text
AI-Based Attendance System
Smart Healthcare using AI
AI Chatbot for Students
```

**Task:** Write a query using a regular expression to find these projects.

**Operator:** `$regex`

---

### 13. Employee Performance Evaluation

The HR department stores employee performance scores:

```javascript
{
  name: "Ravi",
  department: "IT",
  performanceScore: 87
}
```

Employees are considered high performers if their score is  **greater than or equal to 85** .

**Task:** Find all high-performing employees.

Then find employees whose score is  **between 60 and 84** .

**Operators:** `$gte`, `$lt`, `$and`

---

### 14. Inventory Alert Using Multiple Conditions

An e-commerce company wants to identify products requiring immediate restocking.

A product requires restocking when:

* `quantity` is less than 10 **AND**
* `isActive` is `true`

**Task:** Write a query to find all such products.

Then modify the query so that products are selected when:

> quantity is less than 10 **OR** the product is marked as inactive.

**Operators:** `$lt`, `$eq`, `$and`, `$or`

---

### 15. Advanced Student Search

The placement cell wants to shortlist students using the following criteria:

A student should satisfy:

* CGPA ≥ 8.0
* Attendance ≥ 80%
* Branch is either `CSE`, `AI`, or `Data Science`
* The student must have an email address
* The student's name should contain `"an"` anywhere, case-insensitive

**Task:** Write a single MongoDB query to retrieve the shortlisted students.

**Operators involved:**

```text
$gte
$in
$exists
$regex
$options
$and
```
