
# `RAISE_APPLICATION_ERROR` in PL/SQL

`RAISE_APPLICATION_ERROR` is used to generate **custom error messages** and return them to the calling application, procedure, or user.

Unlike `RAISE`, which simply raises an exception, `RAISE_APPLICATION_ERROR` lets you specify:

* A custom error number
* A custom error message

---

## Syntax

```sql
RAISE_APPLICATION_ERROR(
    error_number,
    error_message
);
```

### Rules

* Error number must be between **-20000 and -20999**
* Message can be up to **2048 characters**

---

## Example 1: Age Validation

```sql
DECLARE
    age NUMBER := 15;
BEGIN
    IF age < 18 THEN
        RAISE_APPLICATION_ERROR(
            -20001,
            'Age must be 18 or above.'
        );
    END IF;

    DBMS_OUTPUT.PUT_LINE('Valid Age');
END;
/
```

**Output**

```text
ORA-20001: Age must be 18 or above.
```

---

## Example 2: Bank Withdrawal

```sql
DECLARE
    balance NUMBER := 5000;
    withdraw_amt NUMBER := 7000;
BEGIN
    IF withdraw_amt > balance THEN
        RAISE_APPLICATION_ERROR(
            -20002,
            'Insufficient Balance.'
        );
    END IF;

    balance := balance - withdraw_amt;
END;
/
```

---

## Example 3: Inside a Procedure

```sql
CREATE OR REPLACE PROCEDURE check_salary(
    p_salary NUMBER
)
AS
BEGIN
    IF p_salary < 10000 THEN
        RAISE_APPLICATION_ERROR(
            -20003,
            'Salary cannot be less than 10000.'
        );
    END IF;
END;
/
```

Calling:

```sql
BEGIN
    check_salary(5000);
END;
/
```

Output:

```text
ORA-20003: Salary cannot be less than 10000.
```

---

# Difference Between `RAISE` and `RAISE_APPLICATION_ERROR`

| Feature                      | `RAISE` | `RAISE_APPLICATION_ERROR` |
| ---------------------------- | --------- | --------------------------- |
| Raises exception             | Yes       | Yes                         |
| Custom message               | No        | Yes                         |
| Custom error code            | No        | Yes                         |
| User-friendly errors         | Limited   | Excellent                   |
| Used in procedures/functions | Yes       | Very common                 |

### Using `RAISE`

```sql
DECLARE
    invalid_age EXCEPTION;
BEGIN
    RAISE invalid_age;
END;
/
```

Output:

```text
ORA-06510: PL/SQL: unhandled user-defined exception
```

Notice that Oracle doesn't tell us **why** the exception occurred.

---

### Using `RAISE_APPLICATION_ERROR`

```sql
BEGIN
    RAISE_APPLICATION_ERROR(
        -20001,
        'Age cannot be less than 18.'
    );
END;
/
```

Output:

```text
ORA-20001: Age cannot be less than 18.
```

This is much clearer.

---

# Can We Catch It?

Yes.

```sql
BEGIN
    RAISE_APPLICATION_ERROR(
        -20001,
        'Custom Error'
    );

EXCEPTION
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE(SQLERRM);
END;
/
```

Output:

```text
ORA-20001: Custom Error
```

---

# When Should You Use It?

Use `RAISE_APPLICATION_ERROR` when:

* Validating business rules
* Checking user input
* Creating stored procedures/functions/packages
* Returning meaningful errors to applications

Examples:

* Student age below minimum limit
* Employee salary below company policy
* Insufficient account balance
* Invalid order quantity
* Duplicate registration attempt

### Interview Question

**Why use `RAISE_APPLICATION_ERROR` instead of `RAISE`?**

Because it allows developers to return **meaningful custom error codes and messages**, making debugging and application handling much easier.
