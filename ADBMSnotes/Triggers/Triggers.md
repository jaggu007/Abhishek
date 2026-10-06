## Triggers in PL/SQL

A **trigger** in PL/SQL is a stored program that **automatically executes when a specified event occurs** in the database.

Unlike a procedure, you  **do not call a trigger manually** . Oracle executes it automatically when its triggering event happens.

### 1. Why do we use triggers?

Triggers are commonly used for:

* Automatically maintaining audit information
* Validating data before insertion/update
* Automatically calculating values
* Preventing invalid operations
* Maintaining related tables
* Recording changes to important data

### 2. Basic Syntax

```sql
CREATE OR REPLACE TRIGGER trigger_name
BEFORE INSERT OR UPDATE OR DELETE
ON table_name
FOR EACH ROW
BEGIN
    -- Trigger statements
END;
/
```

The important parts are:

| Part                          | Meaning                              |
| ----------------------------- | ------------------------------------ |
| `CREATE OR REPLACE TRIGGER` | Creates the trigger                  |
| `BEFORE`/`AFTER`          | Specifies when trigger executes      |
| `INSERT/UPDATE/DELETE`      | Specifies the triggering event       |
| `ON table_name`             | Table associated with trigger        |
| `FOR EACH ROW`              | Executes once for every affected row |
| `BEGIN...END`               | Trigger body                         |

---

# 3. Simple Example

Suppose we have an `EMPLOYEE` table:

```sql
CREATE TABLE employee (
    emp_id NUMBER,
    emp_name VARCHAR2(50),
    salary NUMBER
);
```

We want to display a message whenever a new employee is inserted.

```sql
CREATE OR REPLACE TRIGGER trg_employee_insert
AFTER INSERT
ON employee
BEGIN
    DBMS_OUTPUT.PUT_LINE('New employee has been added.');
END;
/
```

Now:

```sql
INSERT INTO employee
VALUES (101, 'Rahul', 30000);
```

The trigger automatically executes.

Output:

```text
New employee has been added.
```

You didn't explicitly call the trigger.

---

# 4. Types of Triggers

Triggers can be classified mainly based on **when** and **what event** causes them to execute.

### Based on timing

**BEFORE Trigger**

Executes before the operation.

```sql
BEFORE INSERT
```

**AFTER Trigger**

Executes after the operation.

```sql
AFTER INSERT
```

**INSTEAD OF Trigger**

Generally used with **views** to perform an operation instead of the normal operation.

---

### Based on event

A trigger can execute when:

```text
INSERT
UPDATE
DELETE
```

For example:

```sql
BEFORE INSERT
AFTER INSERT
BEFORE UPDATE
AFTER UPDATE
BEFORE DELETE
AFTER DELETE
```

You can also combine events:

```sql
AFTER INSERT OR UPDATE OR DELETE
```

---

# 5. Row-Level Trigger

A **row-level trigger** executes once for  **each row affected** .

It uses:

```sql
FOR EACH ROW
```

Example:

```sql
CREATE OR REPLACE TRIGGER trg_salary_check
BEFORE INSERT OR UPDATE
ON employee
FOR EACH ROW
BEGIN
    IF :NEW.salary < 10000 THEN
        RAISE_APPLICATION_ERROR(
            -20001,
            'Salary cannot be less than 10000'
        );
    END IF;
END;
/
```

Now:

```sql
INSERT INTO employee
VALUES (102, 'Amit', 5000);
```

The trigger prevents the insertion.

---

# 6. `:NEW` and `:OLD`

This is one of the  **most important concepts in triggers** .

For row-level triggers, Oracle provides two special records:

### `:NEW`

Represents the  **new value** .

### `:OLD`

Represents the  **old value** .

For example, if salary changes:

```text
Old Salary = 30,000
New Salary = 35,000
```

Then:

```sql
:OLD.salary
```

means:

```text
30000
```

and

```sql
:NEW.salary
```

means:

```text
35000
```

### Availability

| Operation | `:OLD` | `:NEW` |
| --------- | -------- | -------- |
| INSERT    | ❌       | ✅       |
| UPDATE    | ✅       | ✅       |
| DELETE    | ✅       | ❌       |

---

# 7. Practical Example – Salary Audit

Suppose we want to maintain a history whenever an employee's salary changes.

Create an audit table:

```sql
CREATE TABLE salary_audit (
    emp_id NUMBER,
    old_salary NUMBER,
    new_salary NUMBER,
    change_date DATE
);
```

Create the trigger:

```sql
CREATE OR REPLACE TRIGGER trg_salary_audit
AFTER UPDATE OF salary
ON employee
FOR EACH ROW
BEGIN
    INSERT INTO salary_audit
    VALUES (
        :OLD.emp_id,
        :OLD.salary,
        :NEW.salary,
        SYSDATE
    );
END;
/
```

Now:

```sql
UPDATE employee
SET salary = 40000
WHERE emp_id = 101;
```

The trigger automatically inserts:

```text
101 | 30000 | 40000 | current date
```

into `salary_audit`.

---

# 8. BEFORE Trigger for Automatic Values

A very common use of a trigger is automatically generating values.

Suppose:

```sql
CREATE TABLE student (
    student_id NUMBER,
    student_name VARCHAR2(50),
    admission_date DATE
);
```

We want Oracle to automatically store the current date.

```sql
CREATE OR REPLACE TRIGGER trg_student_date
BEFORE INSERT
ON student
FOR EACH ROW
BEGIN
    :NEW.admission_date := SYSDATE;
END;
/
```

Now the user only needs to provide:

```sql
INSERT INTO student(student_id, student_name)
VALUES (1, 'Ravi');
```

The trigger automatically sets:

```text
admission_date = SYSDATE
```

---

# 9. Trigger with Validation

Triggers can also prevent invalid data.

```sql
CREATE OR REPLACE TRIGGER trg_check_salary
BEFORE INSERT OR UPDATE
ON employee
FOR EACH ROW
BEGIN
    IF :NEW.salary <= 0 THEN
        RAISE_APPLICATION_ERROR(
            -20002,
            'Salary must be greater than zero'
        );
    END IF;
END;
/
```

Attempt:

```sql
INSERT INTO employee
VALUES (103, 'Neha', -5000);
```

Oracle raises an error:

```text
ORA-20002: Salary must be greater than zero
```

---

# 10. Statement-Level vs Row-Level Trigger

This distinction is important for exams.

### Statement-Level Trigger

Executes  **once for the entire SQL statement** .

```sql
CREATE OR REPLACE TRIGGER trg_emp_update
AFTER UPDATE
ON employee
BEGIN
    DBMS_OUTPUT.PUT_LINE('Employee table updated');
END;
/
```

If:

```sql
UPDATE employee
SET salary = salary + 1000;
```

updates 50 employees, the trigger executes  **once** .

### Row-Level Trigger

```sql
CREATE OR REPLACE TRIGGER trg_emp_update
AFTER UPDATE
ON employee
FOR EACH ROW
BEGIN
    DBMS_OUTPUT.PUT_LINE('One employee updated');
END;
/
```

If 50 employees are updated, the trigger executes  **50 times** .

---

## 11. Quick Summary

```text
                    TRIGGER
                       |
          +------------+------------+
          |                         |
       Timing                      Event
          |                         |
    +-----+-----+              +----+----+
    |           |              |    |    |
  BEFORE       AFTER         INSERT UPDATE DELETE
    |
  INSTEAD OF
```
