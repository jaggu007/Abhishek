## Functions in PL/SQL

A **function** in PL/SQL is a named block of code that performs a specific task and **must return a value**.

Think of it like:

> **Input → Function → Returned Output**

For example, if we give a function two numbers, it can calculate their sum and return the result.

### 1. Basic Syntax

```sql
CREATE OR REPLACE FUNCTION function_name
(
    parameter1 datatype,
    parameter2 datatype
)
RETURN datatype
IS
    -- variable declarations
BEGIN
    -- statements

    RETURN value;
END;
/
```

The important difference from a **procedure** is:

```sql
RETURN datatype
```

and inside the function:

```sql
RETURN value;
```

---

## 2. Simple Function Example

Let's create a function that adds two numbers:

```sql
CREATE OR REPLACE FUNCTION add_numbers
(
    a NUMBER,
    b NUMBER
)
RETURN NUMBER
IS
BEGIN
    RETURN a + b;
END;
/
```

Now we can call it:

```sql
SELECT add_numbers(10, 20)
FROM dual;
```

Output:

```text
ADD_NUMBERS(10,20)
------------------
30
```

### How it works

```text
add_numbers(10, 20)
        ↓
     10 + 20
        ↓
       30
```

---

## 3. Function with Variables

We can also declare variables inside a function.

```sql
CREATE OR REPLACE FUNCTION calculate_bonus
(
    salary NUMBER
)
RETURN NUMBER
IS
    bonus NUMBER;
BEGIN
    bonus := salary * 0.10;

    RETURN bonus;
END;
/
```

Call it:

```sql
SELECT calculate_bonus(50000)
FROM dual;
```

Output:

```text
5000
```

---

## 4. Function with a Table

Suppose we have:

```sql
CREATE TABLE employee (
    emp_id NUMBER,
    emp_name VARCHAR2(50),
    salary NUMBER
);
```

We can create a function that returns an employee's salary.

```sql
CREATE OR REPLACE FUNCTION get_salary
(
    p_emp_id NUMBER
)
RETURN NUMBER
IS
    v_salary NUMBER;
BEGIN
    SELECT salary
    INTO v_salary
    FROM employee
    WHERE emp_id = p_emp_id;

    RETURN v_salary;
END;
/
```

Call it:

```sql
SELECT get_salary(101)
FROM dual;
```

---

## 5. Function vs Procedure

This is the most important distinction:

| Function                                        | Procedure                                         |
| ----------------------------------------------- | ------------------------------------------------- |
| **Must return a value**                   | Does not have to return a value                   |
| Uses`RETURN datatype`                         | No`RETURN datatype` in declaration              |
| Uses`RETURN value`                            | Can use`RETURN` to exit, but not return a value |
| Can be used in SQL in many situations           | Generally called as a PL/SQL statement            |
| Usually used for calculation/retrieving a value | Usually used to perform an action                 |

For example:

### Function

```sql
CREATE OR REPLACE FUNCTION square_num(n NUMBER)
RETURN NUMBER
IS
BEGIN
    RETURN n * n;
END;
/
```

Usage:

```sql
SELECT square_num(5) FROM dual;
```

Result:

```text
25
```

### Procedure

```sql
CREATE OR REPLACE PROCEDURE print_square(n NUMBER)
IS
BEGIN
    DBMS_OUTPUT.PUT_LINE(n * n);
END;
/
```

Usage:

```sql
BEGIN
    print_square(5);
END;
/
```

Output:

```text
25
```

**Easy way to remember:**

> **Function → gives you something back**
> **Procedure → performs something**

Since you've already covered procedures, the next useful step is **calling functions from PL/SQL, SQL, and procedures**, followed by **functions with `IN`, `OUT`, and `IN OUT` parameters**.
