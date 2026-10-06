In PL/SQL, you can pass user-defined values in several ways depending on where you're running the code.

### 1. Using Substitution Variables (`&`)

Common in SQL*Plus and Oracle SQL Developer.

```sql
DECLARE
    v_num NUMBER;
BEGIN
    v_num := &Enter_Number;

    DBMS_OUTPUT.PUT_LINE('You entered: ' || v_num);
END;
/
```

**Execution:**

```
Enter value for Enter_Number: 25
```

**Output:**

```
You entered: 25
```

---

### 2. Using ACCEPT Command (SQL*Plus)

```sql
ACCEPT p_name CHAR PROMPT 'Enter your name: '

DECLARE
    v_name VARCHAR2(50);
BEGIN
    v_name := '&p_name';

    DBMS_OUTPUT.PUT_LINE('Hello ' || v_name);
END;
/
```

---

### 3. Passing Parameters to a Procedure

```sql
CREATE OR REPLACE PROCEDURE greet_user(
    p_name VARCHAR2
)
IS
BEGIN
    DBMS_OUTPUT.PUT_LINE('Hello ' || p_name);
END;
/
```

Call it:

```sql
BEGIN
    greet_user('Abhishek');
END;
/
```

Output:

```
Hello Abhishek
```

---

### 4. Using Bind Variables

```sql
VARIABLE v_salary NUMBER;

EXEC :v_salary := 50000;

BEGIN
    DBMS_OUTPUT.PUT_LINE('Salary = ' || :v_salary);
END;
/
```

---

### Example: Add Two User-Entered Numbers

```sql
DECLARE
    num1 NUMBER := &num1;
    num2 NUMBER := &num2;
BEGIN
    DBMS_OUTPUT.PUT_LINE('Sum = ' || (num1 + num2));
END;
/
```

When executed:

```
Enter value for num1: 10
Enter value for num2: 20
```

Output:

```
Sum = 30
```

### Interview Point

PL/SQL itself does  **not have a built-in `scanf()`-like input function** . User input is usually taken through:

* Substitution variables (`&`)
* Bind variables (`:`)
* Procedure/Function parameters
* Application front-end (Java, Python, Forms, APEX, etc.) passing values to PL/SQL.

Which environment are you using— **Oracle SQL Developer** ,  **SQL*Plus** , or something else? The exact method can vary slightly.
