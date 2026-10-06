# Cursor in PL/SQL

### A **cursor** is a pointer to the result set returned by a SQL query. It allows PL/SQL to process query results  **one row at a time** .

### Think of a cursor as a bookmark that helps PL/SQL move through the rows returned by a `SELECT` statement.

---

## Why Do We Need a Cursor?

Suppose a query returns multiple employees:

```sql
SELECT emp_id, emp_name
FROM employee;
```

PL/SQL cannot store multiple rows directly into variables using `SELECT INTO`.

To process each row one by one, we use a  **cursor** .

---

# Types of Cursors

## 1. Implicit Cursor

Oracle automatically creates an implicit cursor for:

* INSERT
* UPDATE
* DELETE
* SELECT INTO

Example:

```sql
BEGIN
    UPDATE employee
    SET salary = salary + 1000
    WHERE dept_id = 10;

    DBMS_OUTPUT.PUT_LINE(SQL%ROWCOUNT || ' rows updated');
END;
/
```

Oracle creates and manages the cursor automatically.

---

## 2. Explicit Cursor

When a query returns multiple rows, we create and manage the cursor ourselves.

### Syntax

```sql
DECLARE
    CURSOR cursor_name IS
        SELECT column1, column2
        FROM table_name;
BEGIN
    OPEN cursor_name;

    LOOP
        FETCH cursor_name INTO variable1, variable2;

        EXIT WHEN cursor_name%NOTFOUND;

        -- process data
    END LOOP;

    CLOSE cursor_name;
END;
/
```

---

# Example

Employee table:

| Emp_ID | Emp_Name | Salary |
| ------ | -------- | ------ |
| 101    | Ravi     | 45000  |
| 102    | Amit     | 50000  |
| 103    | Neha     | 55000  |

### Explicit Cursor Example

CREATE TABLE Employees (
    Emp_ID INT PRIMARY KEY,
    Emp_Name VARCHAR(50),
    Salary INT
);

```sql
CREATE TABLE Employees (
    Emp_ID INT PRIMARY KEY,
    Emp_Name VARCHAR(50),
    Salary INT
);

INSERT INTO Employees (Emp_ID, Emp_Name, Salary) VALUES
(101, 'Ravi', 45000),
(102, 'Amit', 50000),
(103, 'Neha', 55000);

// you may use it as normal sql of inside plsql block as u want.
```

```sql
DECLARE
    CURSOR emp_cursor IS 
        SELECT emp_id, emp_name
        FROM employee;

    v_id employee.emp_id%TYPE;
    v_name employee.emp_name%TYPE;
BEGIN
    OPEN emp_cursor;

    LOOP
        FETCH emp_cursor INTO v_id, v_name;

        EXIT WHEN emp_cursor%NOTFOUND;

        DBMS_OUTPUT.PUT_LINE(v_id || ' - ' || v_name);
    END LOOP;

    CLOSE emp_cursor;
END;
/
```

### Output

```text
101 - Ravi
102 - Amit
103 - Neha
```

---

# Cursor Lifecycle

An explicit cursor goes through 4 steps:

### 1. Declare

```sql
CURSOR emp_cursor IS
SELECT * FROM employee;
```

### 2. Open

```sql
OPEN emp_cursor;
```

Oracle executes the query and prepares the result set.

### 3. Fetch

```sql
FETCH emp_cursor INTO emp_record;
```

Retrieves one row at a time.

### 4. Close

```sql
CLOSE emp_cursor;
```

Releases memory.

---

# Cursor Attributes

These provide information about the cursor.

| Attribute     | Description                     |
| ------------- | ------------------------------- |
| `%FOUND`    | Last fetch returned a row       |
| `%NOTFOUND` | Last fetch did not return a row |
| `%ROWCOUNT` | Number of rows fetched          |
| `%ISOPEN`   | Whether cursor is open          |

Example:

```sql
DBMS_OUTPUT.PUT_LINE(emp_cursor%ROWCOUNT);
```

---

# Cursor FOR LOOP (Most Popular)

Oracle automatically handles:

* OPEN
* FETCH
* CLOSE

Example:

```sql
DECLARE
    CURSOR emp_cursor IS
        SELECT emp_id, emp_name
        FROM employee;
BEGIN
    FOR emp_rec IN emp_cursor LOOP
        DBMS_OUTPUT.PUT_LINE(
            emp_rec.emp_id || ' - ' ||
            emp_rec.emp_name
        );
    END LOOP;
END;
/
```

This is shorter and usually preferred.

---

# Cursor with `%ROWTYPE`

```sql
DECLARE
    CURSOR emp_cursor IS
        SELECT *
        FROM employee;

    emp_rec employee%ROWTYPE;
BEGIN
    OPEN emp_cursor;

    LOOP
        FETCH emp_cursor INTO emp_rec;

        EXIT WHEN emp_cursor%NOTFOUND;

        DBMS_OUTPUT.PUT_LINE(
            emp_rec.emp_name || ' ' ||
            emp_rec.salary
        );
    END LOOP;

    CLOSE emp_cursor;
END;
/
```

---

# Parameterized Cursor in PL/SQL

A **parameterized cursor** is an explicit cursor that accepts one or more parameters when it is opened.

It works much like a function that receives input values and uses them inside its query.

---

## Why Use a Parameterized Cursor?

Without parameters:

```sql
CURSOR emp_cursor IS
SELECT *
FROM employee
WHERE dept_id = 10;
```

The department number is fixed.

With a parameterized cursor:

```sql
CURSOR emp_cursor(p_dept_id NUMBER) IS
SELECT *
FROM employee
WHERE dept_id = p_dept_id;
```

Now the same cursor can be used for different departments.

---

# Syntax

```sql
CURSOR cursor_name(parameter datatype) IS
SELECT ...
FROM table_name
WHERE column_name = parameter;
```

---

# Example 1: Single Parameter

```sql
DECLARE
    CURSOR emp_cursor(p_dept_id NUMBER) IS
        SELECT emp_id, emp_name, salary
        FROM employee
        WHERE dept_id = p_dept_id;

    v_emp_id employee.emp_id%TYPE;
    v_emp_name employee.emp_name%TYPE;
    v_salary employee.salary%TYPE;
BEGIN
    OPEN emp_cursor(10);

    LOOP
        FETCH emp_cursor
        INTO v_emp_id, v_emp_name, v_salary;

        EXIT WHEN emp_cursor%NOTFOUND;

        DBMS_OUTPUT.PUT_LINE(
            v_emp_name || ' - ' || v_salary
        );
    END LOOP;

    CLOSE emp_cursor;
END;
/
```

### Output

```text
Ravi - 45000
Amit - 50000
```

(assuming these employees belong to department 10)

---

# Example 2: Reusing the Same Cursor

```sql
DECLARE
    CURSOR emp_cursor(p_dept_id NUMBER) IS
        SELECT emp_name
        FROM employee
        WHERE dept_id = p_dept_id;
BEGIN
    FOR emp_rec IN emp_cursor(10) LOOP
        DBMS_OUTPUT.PUT_LINE(emp_rec.emp_name);
    END LOOP;

    DBMS_OUTPUT.PUT_LINE('-----');

    FOR emp_rec IN emp_cursor(20) LOOP
        DBMS_OUTPUT.PUT_LINE(emp_rec.emp_name);
    END LOOP;
END;
/
```

The same cursor is used for department 10 and department 20.

---

# Example 3: Multiple Parameters

```sql
DECLARE
    CURSOR emp_cursor(
        p_dept_id NUMBER,
        p_salary NUMBER
    ) IS
        SELECT emp_name, salary
        FROM employee
        WHERE dept_id = p_dept_id
        AND salary > p_salary;
BEGIN
    FOR emp_rec IN emp_cursor(10, 40000) LOOP
        DBMS_OUTPUT.PUT_LINE(
            emp_rec.emp_name || ' - ' ||
            emp_rec.salary
        );
    END LOOP;
END;
/
```

This fetches employees from department 10 whose salary is greater than 40,000.

---

# Parameterized Cursor with `%ROWTYPE`

```sql
DECLARE
    CURSOR emp_cursor(p_dept_id NUMBER) IS
        SELECT *
        FROM employee
        WHERE dept_id = p_dept_id;

    emp_rec employee%ROWTYPE;
BEGIN
    OPEN emp_cursor(10);

    LOOP
        FETCH emp_cursor INTO emp_rec;

        EXIT WHEN emp_cursor%NOTFOUND;

        DBMS_OUTPUT.PUT_LINE(emp_rec.emp_name);
    END LOOP;

    CLOSE emp_cursor;
END;
/
```

---

# Important Points

### Parameters are passed when opening the cursor

```sql
OPEN emp_cursor(10);
```

### Not when declaring it

```sql
CURSOR emp_cursor(p_dept_id NUMBER) IS
...
```

### Cursor FOR LOOP automatically passes parameters

```sql
FOR emp_rec IN emp_cursor(10)
LOOP
   ...
END LOOP;
```

No need for `OPEN`, `FETCH`, or `CLOSE`.

# When Should You Use a Cursor?

Use a cursor when:

✅ Query returns multiple rows
✅ Need to process rows one by one
✅ Need custom row-by-row logic

Avoid cursors when:

❌ A single SQL statement can do the job

For example:

Instead of:

```sql
-- Cursor loop updating every employee
```

Prefer:

```sql
UPDATE employee
SET salary = salary + 1000;
```

because SQL operations are usually faster than row-by-row processing.

---
