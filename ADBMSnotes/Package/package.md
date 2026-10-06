# Packages in PL/SQL

A **package** in PL/SQL is a container that groups related **procedures, functions, variables, cursors, and exceptions** together.

Think of a package like a **folder** in which we keep related PL/SQL code.

For example, for an employee system, we can have one package containing:

* Employee insertion procedure
* Employee salary update procedure
* Employee search function
* Employee-related variables

---

## 1. Why do we need Packages?

Suppose we have these separate procedures:

```sql
CREATE PROCEDURE add_employee ...
CREATE PROCEDURE delete_employee ...
CREATE PROCEDURE update_salary ...
CREATE FUNCTION get_salary ...
```

As the application grows, managing all these separately becomes difficult.

Instead, we can group them:

```text
EMPLOYEE_PACKAGE
│
├── add_employee()
├── delete_employee()
├── update_salary()
└── get_salary()
```

This makes the code:

* Organized
* Easier to maintain
* Reusable
* More secure
* Easier to understand

---

# 2. A Package has TWO parts

This is the most important concept.

A PL/SQL package consists of:

### ① Package Specification

It tells  **what is available outside the package** .

Think of it as the **interface/menu** of the package.

### ② Package Body

It contains  **how those things actually work** .

Think of it as the  **implementation** .

```text
             PACKAGE
                │
       ┌────────┴────────┐
       │                 │
 Specification         Body
       │                 │
 What is available     How it works
       │                 │
       └────────┬────────┘
                │
             Database
```

---

# 3. Simple Package Example

Suppose we have:

```sql
CREATE TABLE employee (
    emp_id NUMBER,
    emp_name VARCHAR2(50),
    salary NUMBER
);
```

We want two operations:

1. Add employee
2. Display employee salary

### Step 1: Create Package Specification

```sql
CREATE OR REPLACE PACKAGE employee_pkg AS

    PROCEDURE add_employee(
        p_id NUMBER,
        p_name VARCHAR2,
        p_salary NUMBER
    );

    FUNCTION get_salary(
        p_id NUMBER
    ) RETURN NUMBER;

END employee_pkg;
/
```

Notice that we are only declaring the procedures/functions.

We haven't written their implementation yet.

---

# 4. Create Package Body

Now we define how they work.

```sql
CREATE OR REPLACE PACKAGE BODY employee_pkg AS

    PROCEDURE add_employee(
        p_id NUMBER,
        p_name VARCHAR2,
        p_salary NUMBER
    )
    IS
    BEGIN
        INSERT INTO employee
        VALUES (p_id, p_name, p_salary);

        DBMS_OUTPUT.PUT_LINE('Employee inserted');
    END add_employee;


    FUNCTION get_salary(
        p_id NUMBER
    ) RETURN NUMBER
    IS
        v_salary NUMBER;
    BEGIN
        SELECT salary
        INTO v_salary
        FROM employee
        WHERE emp_id = p_id;

        RETURN v_salary;
    END get_salary;

END employee_pkg;
/
```

Now our package is ready.

---

# 5. How to use the Package?

We don't create an object of the package.

We simply use:

```sql
package_name.procedure_name
```

For example:

```sql
BEGIN
    employee_pkg.add_employee(101, 'Ravi', 45000);
END;
/
```

To call the function:

```sql
DECLARE
    v_salary NUMBER;
BEGIN
    v_salary := employee_pkg.get_salary(101);

    DBMS_OUTPUT.PUT_LINE('Salary = ' || v_salary);
END;
/
```

Output:

```text
Employee inserted

Salary = 45000
```

---

# 6. Public vs Private Members

This is another **very important advantage** of packages.

Anything declared in the **package specification** is generally accessible from outside.

For example:

```sql
CREATE OR REPLACE PACKAGE employee_pkg AS

    PROCEDURE add_employee(
        p_id NUMBER,
        p_name VARCHAR2,
        p_salary NUMBER
    );

END employee_pkg;
/
```

`add_employee` is publicly available.

But suppose we create a variable only inside the package body:

```sql
CREATE OR REPLACE PACKAGE BODY employee_pkg AS

    v_count NUMBER := 0;

    PROCEDURE add_employee(
        p_id NUMBER,
        p_name VARCHAR2,
        p_salary NUMBER
    )
    IS
    BEGIN
        INSERT INTO employee
        VALUES (p_id, p_name, p_salary);

        v_count := v_count + 1;
    END add_employee;

END employee_pkg;
/
```

`v_count` is  **private** .

You cannot do:

```sql
BEGIN
    employee_pkg.v_count := 10;
END;
/
```

because `v_count` isn't declared in the specification.

So:

```text
Package Specification
        ↓
     PUBLIC
        ↓
Can be accessed from outside


Package Body
        ↓
    PRIVATE
        ↓
Internal implementation
```

---

# 7. Package Variables

A package can also contain variables.

For example:

```sql
CREATE OR REPLACE PACKAGE employee_pkg AS

    v_company_name VARCHAR2(100) := 'ABC Technologies';

    PROCEDURE add_employee(
        p_id NUMBER,
        p_name VARCHAR2,
        p_salary NUMBER
    );

END employee_pkg;
/
```

Now we can access the variable:

```sql
BEGIN
    DBMS_OUTPUT.PUT_LINE(employee_pkg.v_company_name);
END;
/
```

Output:

```text
ABC Technologies
```

---

# 8. Package can contain many things

A package can contain:

```text
Package
│
├── Variables
├── Constants
├── Procedures
├── Functions
├── Cursors
├── Exceptions
└── Types
```

For example:

```sql
CREATE OR REPLACE PACKAGE employee_pkg AS

    v_company VARCHAR2(100) := 'ABC Ltd';

    PROCEDURE add_employee(
        p_id NUMBER,
        p_name VARCHAR2,
        p_salary NUMBER
    );

    PROCEDURE delete_employee(
        p_id NUMBER
    );

    FUNCTION get_salary(
        p_id NUMBER
    ) RETURN NUMBER;

END employee_pkg;
/
```

All these related operations are now grouped together.

---

# 9. Package vs Procedure

A common question is:

### Procedure

A procedure is a  **single program unit** .

```text
PROCEDURE
    ↓
One operation
```

### Package

A package is a  **collection of related program units** .

```text
PACKAGE
   │
   ├── Procedure
   ├── Procedure
   ├── Function
   ├── Cursor
   └── Variables
```

So, a package is  **not a replacement for a procedure** .

Rather, a package can  **contain procedures and functions** .

---

# 10. Package vs Package Body

| Package Specification         | Package Body              |
| ----------------------------- | ------------------------- |
| Declares public members       | Implements them           |
| Visible outside               | Usually internal          |
| Defines what package provides | Defines how it works      |
| Similar to interface          | Similar to implementation |

A simple way to remember:

> **Specification = WHAT**
> **Body = HOW**

---
