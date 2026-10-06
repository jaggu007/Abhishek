**Evaluation Operators**. These are particularly useful for **pattern matching, text searching, and mathematical evaluation** in MongoDB.

The most important ones for beginners are:

| Operator   | Purpose                                                              |
| ---------- | -------------------------------------------------------------------- |
| `$regex` | Pattern matching in strings                                          |
| `$text`  | Text search using a text index                                       |
| `$expr`  | Compare/evaluate expressions between fields                          |
| `$mod`   | Perform modulo/divisibility matching                                 |
| `$where` | JavaScript-based condition — generally avoid in modern applications |

For your current student dataset, **`$regex` and `$expr`** are especially useful.

---

# 1. `$regex` — Pattern Matching

`$regex` is used to search for a particular **pattern inside a string**.

Think of it as similar to SQL:

```sql
LIKE
```

### Basic syntax

```javascript
db.students.find({
    field: { $regex: "pattern" }
})
```

---

## Example 1 — Names starting with A

```javascript
db.students.find({
    name: { $regex: "^A" }
})
```

Here:

```text
^A
```

means **starts with A**.

For example:

```text
Aarav Sharma
Ananya Gupta
Aditya Kumar
Arjun Mehta
```

---

## Example 2 — Names ending with "a"

```javascript
db.students.find({
    name: { $regex: "a$" }
})
```

Here:

```text
$
```

means **end of the string**.

---

## Example 3 — Names containing "an"

```javascript
db.students.find({
    name: { $regex: "an" }
})
```

This searches for `"an"` anywhere in the name.

---

# 2. Case-Insensitive Search

By default, regex matching is case-sensitive.

Suppose we want to find names containing `"a"` regardless of uppercase/lowercase.

```javascript
db.students.find({
    name: {
        $regex: "a",
        $options: "i"
    }
})
```

`i` means **case-insensitive**.

So:

```text
A
a
```

are treated the same.

---

# 3. `$options`

`$options` modifies how `$regex` behaves.

### Case-insensitive

```javascript
db.students.find({
    name: {
        $regex: "^a",
        $options: "i"
    }
})
```

Finds names beginning with **A or a**.

---

# 4. `$regex` with Branch

Find students whose branch starts with `"C"`:

```javascript
db.students.find({
    branch: {
        $regex: "^C"
    }
})
```

This will match:

```text
CSE
```

---

# 5. `$regex` with City

Find students whose city contains `"no"`:

```javascript
db.students.find({
    city: {
        $regex: "no",
        $options: "i"
    }
})
```

This can match:

```text
Noida
```

---

# 6. `$expr`

`$expr` allows you to use **expressions inside a query**.

This becomes particularly useful when you want to compare **one field with another field**.

For example, suppose we add:

```javascript
{
    name: "Aarav",
    marks: 85,
    passingMarks: 40
}
```

We can ask:

### Find students whose marks are greater than passing marks

```javascript
db.students.find({
    $expr: {
        $gt: ["$marks", "$passingMarks"]
    }
})
```

Notice the `$` before the field names:

```javascript
"$marks"
"$passingMarks"
```

This tells MongoDB that these are **field references**.

---

# 7. `$mod`

`$mod` is used when we want to find values that produce a particular remainder after division.

Syntax:

```javascript
{
    field: {
        $mod: [divisor, remainder]
    }
}
```

### Example

Find students whose age is an even number:

```javascript
db.students.find({
    age: {
        $mod: [2, 0]
    }
})
```

Meaning:

```text
age ÷ 2 → remainder 0
```

Therefore:

```text
18 ✓
20 ✓
22 ✓
19 ✗
21 ✗
```

---

## Another Example

Find students whose age is odd:

```javascript
db.students.find({
    age: {
        $mod: [2, 1]
    }
})
```

---

# 8. Combining Evaluation Operators

You can combine `$regex` with other query conditions.

### Example

Find CSE students whose name starts with `"A"`.

```javascript
db.students.find({
    branch: "CSE",
    name: {
        $regex: "^A"
    }
})
```

---

### Example

Find students from Delhi whose name contains `"a"`.

```javascript
db.students.find({
    city: "Delhi",
    name: {
        $regex: "a",
        $options: "i"
    }
})
```

---

# Practice Questions

Try these yourself first:

### Basic

1. Find students whose name starts with `"S"`.
2. Find students whose name ends with `"n"`.
3. Find students whose name contains `"a"`.
4. Find students whose city starts with `"D"`.
5. Find students whose branch starts with `"A"`.

### Moderate

6. Find students whose name contains `"ra"` irrespective of case.
7. Find students from Delhi whose name starts with `"R"`.
8. Find students from CSE whose name contains `"a"`.
9. Find students whose age is an even number.
10. Find students whose age is an odd number.

### Advanced

11. Find students from CSE or IT whose name starts with `"A"`.
12. Find students whose name contains `"a"` and CGPA is greater than 8.5.
13. Find students from Delhi whose name starts with `"A"` or `"R"`.
14. Find students whose city contains `"a"` irrespective of case and CGPA is at least 8.0.
15. Find students whose age is even and CGPA is greater than 8.5.

---
