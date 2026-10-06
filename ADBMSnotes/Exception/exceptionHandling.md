# Exception Handling in PL/SQL

Exception handling in PL/SQL is used to handle runtime errors gracefully without abruptly terminating the program.

An **exception** is an condition that occurs during program execution, such as:

* Division by zero
* No data found
* Too many rows returned
* Invalid data conversion

---

## Basic Syntax

```sql
DECLARE
    -- Variable declarations
BEGIN
    -- Executable statements

EXCEPTION
    -- Exception handling code
END;
/
```

---

## Example 1: Handling Division by Zero

```sql
DECLARE
    num1 NUMBER := 100;
    num2 NUMBER := 0;
    result NUMBER;
BEGIN
    result := num1 / num2;

    DBMS_OUTPUT.PUT_LINE('Result: ' || result);

EXCEPTION
    WHEN ZERO_DIVIDE THEN
        DBMS_OUTPUT.PUT_LINE('Cannot divide by zero.');
END;
/
```

**Output:**

```
Cannot divide by zero.
```

---

# Types of Exceptions

## 1. Predefined Exceptions

These are already defined by Oracle.

| Exception          | Description                       |
| ------------------ | --------------------------------- |
| `NO_DATA_FOUND`  | SELECT INTO returns no rows       |
| `TOO_MANY_ROWS`  | SELECT INTO returns multiple rows |
| `ZERO_DIVIDE`    | Division by zero                  |
| `VALUE_ERROR`    | Numeric or conversion error       |
| `INVALID_NUMBER` | Invalid number conversion         |

---

### Example: NO_DATA_FOUND

```sql
DECLARE
    emp_name employee.emp_name%TYPE;
BEGIN
    SELECT emp_name
    INTO emp_name
    FROM employee
    WHERE emp_id = 999;

    DBMS_OUTPUT.PUT_LINE(emp_name);

EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('Employee not found.');
END;
/
```

---

### Example: TOO_MANY_ROWS

```sql
DECLARE
    v_name employee.emp_name%TYPE;
BEGIN
    SELECT emp_name
    INTO v_name
    FROM employee;

EXCEPTION
    WHEN TOO_MANY_ROWS THEN
        DBMS_OUTPUT.PUT_LINE('More than one row returned.');
END;
/
```

---

## 2. User-Defined Exceptions

You can create your own exceptions.

### Example

```sql
DECLARE
    salary NUMBER := 3000;

    low_salary EXCEPTION;
BEGIN
    IF salary < 5000 THEN
        RAISE low_salary;
    END IF;

EXCEPTION
    WHEN low_salary THEN
        DBMS_OUTPUT.PUT_LINE('Salary is below minimum limit.');
END;
/
```

---

## 3. OTHERS Exception

Used to catch any exception not explicitly handled.

```sql
DECLARE
    result NUMBER;
BEGIN
    result := 100 / 0;

EXCEPTION
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('An error occurred.');
END;
/
```

---

## Getting Error Details

Use SQLCODE and SQLERRM.

```sql
DECLARE
    result NUMBER;
BEGIN
    result := 100 / 0;

EXCEPTION
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Error Code: ' || SQLCODE);
        DBMS_OUTPUT.PUT_LINE('Error Message: ' || SQLERRM);
END;
/
```

**Possible Output**

```
Error Code: -1476
Error Message: ORA-01476: divisor is equal to zero
```

---

# Exception Propagation

If an exception is not handled in the current block, it propagates to the enclosing block.

```sql
BEGIN
    BEGIN
        DBMS_OUTPUT.PUT_LINE(100/0);
    END;

EXCEPTION
    WHEN ZERO_DIVIDE THEN
        DBMS_OUTPUT.PUT_LINE('Handled in outer block.');
END;
/
```

---

# RAISE Statement

Used to explicitly raise an exception.

```sql
DECLARE
    invalid_age EXCEPTION;
    age NUMBER := 15;
BEGIN
    IF age < 18 THEN
        RAISE invalid_age;
    END IF;

EXCEPTION
    WHEN invalid_age THEN
        DBMS_OUTPUT.PUT_LINE('Age must be 18 or above.');
END;
/
```

---

# Interview Question

### What is the difference between an error and an exception?

* **Error:** Problem that occurs during execution (e.g., divide by zero).
* **Exception:** PL/SQL mechanism used to detect and handle that error.

---

# Quick Summary

```text
BEGIN
    -- Normal code
EXCEPTION
    WHEN predefined_exception THEN
        -- Handle predefined exception

    WHEN user_defined_exception THEN
        -- Handle custom exception

    WHEN OTHERS THEN
        -- Handle remaining exceptions
END;
```

For beginners, the most important exceptions to remember are:

* `NO_DATA_FOUND`
* `TOO_MANY_ROWS`
* `ZERO_DIVIDE`
* `VALUE_ERROR`
* `OTHERS`

Correct. **PL/SQL does not use `try` and `catch` keywords** like Java, C#, JavaScript, or Python.

Instead, PL/SQL uses the **`EXCEPTION` block**.

### Java Example

```java
try {
    int result = 100 / 0;
}
catch (ArithmeticException e) {
    System.out.println("Cannot divide by zero");
}
```

### Equivalent PL/SQL

```sql
DECLARE
    result NUMBER;
BEGIN
    result := 100 / 0;

EXCEPTION
    WHEN ZERO_DIVIDE THEN
        DBMS_OUTPUT.PUT_LINE('Cannot divide by zero');
END;
/
```

### Mapping

| Java/C#         | PL/SQL                 |
| --------------- | ---------------------- |
| `try`         | `BEGIN`              |
| `catch`       | `EXCEPTION ... WHEN` |
| `finally`     | No direct equivalent   |
| `throw`       | `RAISE`              |
| `Exception e` | `WHEN OTHERS`        |

---

### Multiple Catch Blocks

**Java**

```java
try {
    // code
}
catch (ArithmeticException e) {
}
catch (NullPointerException e) {
}
catch (Exception e) {
}
```

**PL/SQL**

```sql
BEGIN
    -- code

EXCEPTION
    WHEN ZERO_DIVIDE THEN
        DBMS_OUTPUT.PUT_LINE('Division by zero');

    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('No record found');

    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Some other error');
END;
/
```

---

### Raising an Exception

**Java**

```java
throw new Exception("Invalid Age");
```

**PL/SQL**

```sql
DECLARE
    invalid_age EXCEPTION;
BEGIN
    RAISE invalid_age;

EXCEPTION
    WHEN invalid_age THEN
        DBMS_OUTPUT.PUT_LINE('Invalid Age');
END;
/
```

### Why no `try-catch` in PL/SQL?

PL/SQL was designed before Java became popular. Oracle chose a different syntax:

```sql
BEGIN
    -- normal code
EXCEPTION
    -- error handling code
END;
```

The `EXCEPTION` section is considered part of the block itself, rather than a separate `catch` construct.

A useful way to think about it:

```text
Java                     PL/SQL
----------------------------------------
try {                    BEGIN
   code                     code
}                        EXCEPTION
catch(...) {                WHEN ...
   handler                  handler
}                        END;
```

So whenever you see a `BEGIN ... EXCEPTION ... END` block in PL/SQL, mentally read it as a `try-catch` block.
