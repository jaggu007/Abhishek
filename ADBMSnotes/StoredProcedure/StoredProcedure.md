# Stored Procedures in PL/SQL

A **Stored Procedure** is a named PL/SQL block that is stored in the Oracle database and can be executed whenever needed.

It helps in:

* Reusing code
* Improving performance
* Reducing network traffic
* Centralizing business logic

## Syntax

```sql
CREATE OR REPLACE PROCEDURE procedure_name
IS
BEGIN
    -- executable statements
END;
/
```

---

## Example 1: Simple Procedure

```sql
CREATE OR REPLACE PROCEDURE greet_user
IS
BEGIN
    DBMS_OUTPUT.PUT_LINE('Welcome to PL/SQL');
END;
/
```

### Execute

```sql
EXEC greet_user;
```

or

```sql
BEGIN
    greet_user;
END;
/
```

### Output

```
Welcome to PL/SQL
```

---

# Procedure with Parameters

Parameters allow passing values to a procedure.

## IN Parameter

Used to receive values from the caller.

```sql
CREATE OR REPLACE PROCEDURE show_square(
    num IN NUMBER
)
IS
BEGIN
    DBMS_OUTPUT.PUT_LINE('Square = ' || (num * num));
END;
/
```

### Execute

```sql
EXEC show_square(5);
```

### Output

```
Square = 25
```

---

# OUT Parameter

Used to return values to the caller.

```sql
CREATE OR REPLACE PROCEDURE get_bonus(
    salary IN NUMBER,
    bonus OUT NUMBER
)
IS
BEGIN
    bonus := salary * 0.10;
END;
/
```

### Calling Procedure

```sql
DECLARE
    emp_bonus NUMBER;
BEGIN
    get_bonus(50000, emp_bonus);

    DBMS_OUTPUT.PUT_LINE('Bonus = ' || emp_bonus);
END;
/
```

### Output

```
Bonus = 5000
```

---

# IN OUT Parameter

Acts as both input and output.

```sql
CREATE OR REPLACE PROCEDURE increment_value(
    num IN OUT NUMBER
)
IS
BEGIN
    num := num + 1;
END;
/
```

### Calling Procedure

```sql
DECLARE
    value NUMBER := 10;
BEGIN
    increment_value(value);

    DBMS_OUTPUT.PUT_LINE(value);
END;
/
```

### Output

```
11
```

---

# Removing a Procedure

```sql
DROP PROCEDURE add_employee;
```

---

To create a table from a stored procedure in PL/SQL, you must use **`EXECUTE IMMEDIATE`** because `CREATE TABLE` is a DDL statement.

### Example

```sql
CREATE OR REPLACE PROCEDURE create_student_table
IS
BEGIN
    EXECUTE IMMEDIATE '
        CREATE TABLE student (
            student_id NUMBER PRIMARY KEY,
            student_name VARCHAR2(50),
            marks NUMBER
        )';

    DBMS_OUTPUT.PUT_LINE('Table STUDENT created successfully.');
END;
/
```

### Execute the Procedure

```sql
EXEC create_student_table;
```

or

```sql
BEGIN
    create_student_table;
END;
/
```

### Verify the Table

```sql
DESC student;
```

or

```sql
SELECT table_name
FROM user_tables
WHERE table_name = 'STUDENT';
```

Once the table is created, we can create another stored procedure to **insert values into the table**.

Suppose we have this table:

```sql
CREATE TABLE student (
    student_id NUMBER PRIMARY KEY,
    student_name VARCHAR2(50),
    marks NUMBER
);
```

## 1. Create Procedure for INSERT

```sql
CREATE OR REPLACE PROCEDURE add_student(
    p_id    IN NUMBER,
    p_name  IN VARCHAR2,
    p_marks IN NUMBER
)
IS
BEGIN
    INSERT INTO student(student_id, student_name, marks)
    VALUES(p_id, p_name, p_marks);

    DBMS_OUTPUT.PUT_LINE('Student inserted successfully.');
END;
/
```

Here:

* `p_id` → receives student ID
* `p_name` → receives student name
* `p_marks` → receives marks
* All three are `IN` parameters because they are **input values**.

---

## 2. Execute the Procedure

We can directly pass values:

```sql
EXEC add_student(101, 'Ravi', 85);
```

Another student:

```sql
EXEC add_student(102, 'Priya', 92);
```

Another:

```sql
EXEC add_student(103, 'Amit', 78);
```

---

## 3. Check the Table

```sql
SELECT * FROM student;
```

Output:

```text
STUDENT_ID   STUDENT_NAME   MARKS
----------   ------------   -----
101          Ravi           85
102          Priya          92
103          Amit           78
```
