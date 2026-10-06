# SELECT INTO Statement in PL/SQL (Beginner Friendly)

The `SELECT INTO` statement is used when you want to  **fetch data from a database table and store it into PL/SQL variables** .

Think of it like this:

> SQL retrieves data from a table.
> PL/SQL stores that retrieved data into variables for further processing.

---

## Syntax

```sql
SELECT column_name
INTO variable_name
FROM table_name
WHERE condition;
```

Or for multiple columns:

```sql
SELECT column1, column2
INTO variable1, variable2
FROM table_name
WHERE condition;
```

---

## Why Do We Need SELECT INTO?

Suppose you have an employee table:

| emp_id | emp_name | salary |
| ------ | -------- | ------ |
| 101    | Ravi     | 45000  |
| 102    | Amit     | 50000  |

If you want to display Ravi's salary inside a PL/SQL block, first you must fetch it into a variable.

```sql
DECLARE
    v_salary NUMBER;
BEGIN
    SELECT salary
    INTO v_salary
    FROM employee
    WHERE emp_id = 101;

    DBMS_OUTPUT.PUT_LINE('Salary = ' || v_salary);
END;
/
```

**Output:**

```
Salary = 45000
```

---

## Example 1: Fetch One Column

```sql
DECLARE
    v_name VARCHAR2(50);
BEGIN
    SELECT emp_name
    INTO v_name
    FROM employee
    WHERE emp_id = 101;

    DBMS_OUTPUT.PUT_LINE('Employee Name: ' || v_name);
END;
/
```

---

## Example 2: Fetch Multiple Columns

```sql
DECLARE
    v_name   VARCHAR2(50);
    v_salary NUMBER;
BEGIN
    SELECT emp_name, salary
    INTO v_name, v_salary
    FROM employee
    WHERE emp_id = 101;

    DBMS_OUTPUT.PUT_LINE(v_name || ' earns ' || v_salary);
END;
/
```

**Output:**

```
Ravi earns 45000
```

---

## Example 3: Using %TYPE

Instead of manually specifying data types:

```sql
DECLARE
    v_salary employee.salary%TYPE;
BEGIN
    SELECT salary
    INTO v_salary
    FROM employee
    WHERE emp_id = 101;

    DBMS_OUTPUT.PUT_LINE(v_salary);
END;
/
```

This automatically uses the same datatype as the `salary` column.

---

## Example 4: Using %ROWTYPE

To fetch an entire row:

```sql
DECLARE
    emp_record employee%ROWTYPE;
BEGIN
    SELECT *
    INTO emp_record
    FROM employee
    WHERE emp_id = 101;

    DBMS_OUTPUT.PUT_LINE(emp_record.emp_name);
    DBMS_OUTPUT.PUT_LINE(emp_record.salary);
END;
/
```

---

# Important Rule

`SELECT INTO` must return  **exactly one row** .

### Case 1: No Row Found

```sql
SELECT salary
INTO v_salary
FROM employee
WHERE emp_id = 999;
```

Error:

```
NO_DATA_FOUND
```

Because employee 999 does not exist.

---

### Case 2: Multiple Rows Found

```sql
SELECT salary
INTO v_salary
FROM employee;
```

Error:

```
TOO_MANY_ROWS
```

Because multiple employees exist.

---

## Handling Exceptions

```sql
DECLARE
    v_salary NUMBER;
BEGIN
    SELECT salary
    INTO v_salary
    FROM employee
    WHERE emp_id = 999;

    DBMS_OUTPUT.PUT_LINE(v_salary);

EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('Employee not found');

    WHEN TOO_MANY_ROWS THEN
        DBMS_OUTPUT.PUT_LINE('More than one employee found');
END;
/
```

---

# Is WHERE Clause Mandatory?

**No.**

Syntax-wise, the `WHERE` clause is optional.

```sql
SELECT COUNT(*)
INTO v_count
FROM employee;
```

This works because `COUNT(*)` always returns exactly one row.

However, when selecting normal column values, a `WHERE` clause is usually used to ensure only **one row** is returned.

---

# SELECT INTO vs Normal SQL

### Normal SQL

```sql
SELECT salary
FROM employee
WHERE emp_id = 101;
```

Result is displayed on the screen.

---

### PL/SQL SELECT INTO

```sql
SELECT salary
INTO v_salary
FROM employee
WHERE emp_id = 101;
```

Result is stored in a variable and can be used later in the program.

---

# Quick Summary

* `SELECT INTO` fetches data from a table into PL/SQL variables.
* It must return  **exactly one row** .
* No row → `NO_DATA_FOUND`.
* Multiple rows → `TOO_MANY_ROWS`.
* Can fetch:
  * Single column
  * Multiple columns
  * Entire row using `%ROWTYPE`
* `WHERE` clause is optional, but often used to ensure a single row is returned.

### Memory Trick

```text
SELECT  -> Get data from table
INTO    -> Store data into variables
FROM    -> Specify table
WHERE   -> Choose the row(s)
```
