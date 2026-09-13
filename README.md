# 🟠 Oracle Database — Beginner to Advanced

> A complete Oracle Database learning guide from **Beginner → Intermediate → Advanced**, with practical SQL, PL/SQL, database design, transactions, indexes, views, procedures, functions, triggers, packages, cursors, analytic functions, and more.

**Languages:** English 🇬🇧 + Khmer 🇰🇭
**Database:** Oracle Database
**SQL:** Oracle SQL
**Programming:** PL/SQL

---

## 📚 Table of Contents

* [1. What is Oracle Database?](#1-what-is-oracle-database)
* [2. Oracle Database Architecture](#2-oracle-database-architecture)
* [3. Installing Oracle](#3-installing-oracle)
* [4. SQL Basics](#4-sql-basics)
* [5. Create Database Objects](#5-create-database-objects)
* [6. Data Types](#6-data-types)
* [7. INSERT](#7-insert)
* [8. SELECT](#8-select)
* [9. WHERE](#9-where)
* [10. ORDER BY](#10-order-by)
* [11. DISTINCT](#11-distinct)
* [12. UPDATE](#12-update)
* [13. DELETE](#13-delete)
* [14. Operators](#14-operators)
* [15. Aggregate Functions](#15-aggregate-functions)
* [16. GROUP BY](#16-group-by)
* [17. HAVING](#17-having)
* [18. String Functions](#18-string-functions)
* [19. Number Functions](#19-number-functions)
* [20. Date Functions](#20-date-functions)
* [21. CASE](#21-case)
* [22. JOINS](#22-joins)
* [23. Subqueries](#23-subqueries)
* [24. Common Table Expressions](#24-common-table-expressions)
* [25. Constraints](#25-constraints)
* [26. ALTER TABLE](#26-alter-table)
* [27. Views](#27-views)
* [28. Sequences](#28-sequences)
* [29. Identity Columns](#29-identity-columns)
* [30. Indexes](#30-indexes)
* [31. Transactions](#31-transactions)
* [32. MERGE](#32-merge)
* [33. Set Operators](#33-set-operators)
* [34. NULL Handling](#34-null-handling)
* [35. Oracle ROWID and ROWNUM](#35-oracle-rowid-and-rownum)
* [36. Pagination](#36-pagination)
* [37. Analytic Functions](#37-analytic-functions)
* [38. Window Functions](#38-window-functions)
* [39. PL/SQL](#39-plsql)
* [40. Variables](#40-variables)
* [41. IF / ELSIF / ELSE](#41-if--elsif--else)
* [42. Loops](#42-loops)
* [43. Cursors](#43-cursors)
* [44. Exceptions](#44-exceptions)
* [45. Procedures](#45-procedures)
* [46. Functions](#46-functions)
* [47. Parameters](#47-parameters)
* [48. Triggers](#48-triggers)
* [49. Packages](#49-packages)
* [50. Dynamic SQL](#50-dynamic-sql)
* [51. Bulk Processing](#51-bulk-processing)
* [52. JSON](#52-json)
* [53. Database Design](#53-database-design)
* [54. Normalization](#54-normalization)
* [55. Security](#55-security)
* [56. Performance](#56-performance)
* [57. Useful Oracle Commands](#57-useful-oracle-commands)
* [58. Complete Mini Project](#58-complete-mini-project)
* [59. Best Practices](#59-best-practices)
* [60. Learning Roadmap](#60-learning-roadmap)

---

# 1. What is Oracle Database?

## English

**Oracle Database** is a relational database management system (RDBMS) developed by Oracle.

It is commonly used for:

* Banking systems
* Enterprise applications
* ERP systems
* Government systems
* E-commerce
* Financial systems
* Large-scale backend applications

Oracle stores data inside:

```text
Database
   ↓
Schema
   ↓
Tables
   ↓
Rows
   ↓
Columns
```

## Khmer

**Oracle Database** គឺជា RDBMS ដែលប្រើសម្រាប់រក្សាទុក និងគ្រប់គ្រងទិន្នន័យ។

វាត្រូវបានប្រើច្រើនក្នុង៖

* ប្រព័ន្ធធនាគារ
* Enterprise Application
* ERP
* E-commerce
* Financial System
* Government System
* Backend Application ធំៗ

---

# 2. Oracle Database Architecture

Basic structure:

```text
Oracle Database
│
├── Instance
│   ├── Memory
│   └── Background Processes
│
└── Database
    ├── Data Files
    ├── Control Files
    ├── Redo Log Files
    └── Tablespaces
        └── Schema
            ├── Tables
            ├── Views
            ├── Indexes
            ├── Procedures
            ├── Functions
            └── Packages
```

### Important Terms

| Term      | Meaning                                    |
| --------- | ------------------------------------------ |
| Database  | Collection of database files               |
| Instance  | Memory + background processes              |
| Schema    | Objects owned by a database user           |
| Table     | Stores structured data                     |
| View      | Virtual table based on a query             |
| Index     | Helps improve query performance            |
| Sequence  | Generates numeric values                   |
| Procedure | Stored PL/SQL program                      |
| Function  | Stored PL/SQL program that returns a value |
| Trigger   | Automatically executed PL/SQL              |
| Package   | Collection of related PL/SQL objects       |

---

# 3. Installing Oracle

Common tools:

* Oracle Database
* Oracle SQL Developer
* Oracle SQLcl
* Oracle Enterprise Manager

After installation, connect using a client such as SQL Developer.

Example connection:

```text
Username: system
Password: your_password
Host: localhost
Port: 1521
Service Name: FREE
```

> The service name depends on your Oracle installation.

---

# 4. SQL Basics

SQL means:

**Structured Query Language**

The major SQL categories are:

```text
DDL
├── CREATE
├── ALTER
├── DROP
└── TRUNCATE

DML
├── INSERT
├── UPDATE
├── DELETE
└── MERGE

DQL
└── SELECT

DCL
├── GRANT
└── REVOKE

TCL
├── COMMIT
├── ROLLBACK
└── SAVEPOINT
```

---

# 5. Create Database Objects

## Create a Table

```sql
CREATE TABLE employees (
    employee_id NUMBER,
    first_name VARCHAR2(50),
    last_name VARCHAR2(50),
    email VARCHAR2(100),
    salary NUMBER(10, 2),
    hire_date DATE
);
```

### Khmer

`CREATE TABLE` ប្រើសម្រាប់បង្កើត Table ថ្មី។

* `NUMBER` = លេខ
* `VARCHAR2` = អក្សរ
* `DATE` = កាលបរិច្ឆេទ

---

# 6. Data Types

Common Oracle data types:

```text
NUMBER
VARCHAR2
CHAR
DATE
TIMESTAMP
CLOB
BLOB
BOOLEAN
```

Example:

```sql
CREATE TABLE products (
    product_id NUMBER,
    product_name VARCHAR2(100),
    price NUMBER(10, 2),
    quantity NUMBER,
    created_at TIMESTAMP,
    description CLOB
);
```

---

# 7. INSERT

Insert one row:

```sql
INSERT INTO employees (
    employee_id,
    first_name,
    last_name,
    email,
    salary,
    hire_date
)
VALUES (
    1,
    'Dara',
    'Sok',
    'dara@example.com',
    1200.00,
    DATE '2025-01-10'
);
```

Insert multiple rows:

```sql
INSERT ALL
    INTO employees (
        employee_id,
        first_name,
        last_name,
        email,
        salary,
        hire_date
    )
    VALUES (
        2,
        'Sopheak',
        'Kim',
        'sopheak@example.com',
        1500,
        DATE '2025-02-01'
    )

    INTO employees (
        employee_id,
        first_name,
        last_name,
        email,
        salary,
        hire_date
    )
    VALUES (
        3,
        'Vanna',
        'Chea',
        'vanna@example.com',
        1800,
        DATE '2025-03-01'
    )
SELECT 1 FROM dual;
```

Then:

```sql
COMMIT;
```

### Khmer

`INSERT` ប្រើសម្រាប់បញ្ចូលទិន្នន័យថ្មីទៅក្នុង Table។

---

# 8. SELECT

Select everything:

```sql
SELECT *
FROM employees;
```

Select specific columns:

```sql
SELECT
    employee_id,
    first_name,
    salary
FROM employees;
```

Alias:

```sql
SELECT
    first_name AS name,
    salary AS monthly_salary
FROM employees;
```

---

# 9. WHERE

Find employees with salary greater than 1500:

```sql
SELECT *
FROM employees
WHERE salary > 1500;
```

Multiple conditions:

```sql
SELECT *
FROM employees
WHERE salary > 1000
  AND salary < 2000;
```

OR:

```sql
SELECT *
FROM employees
WHERE first_name = 'Dara'
   OR first_name = 'Vanna';
```

---

# 10. ORDER BY

Ascending:

```sql
SELECT *
FROM employees
ORDER BY salary ASC;
```

Descending:

```sql
SELECT *
FROM employees
ORDER BY salary DESC;
```

Multiple sorting:

```sql
SELECT *
FROM employees
ORDER BY salary DESC, first_name ASC;
```

---

# 11. DISTINCT

Remove duplicate values:

```sql
SELECT DISTINCT salary
FROM employees;
```

Multiple columns:

```sql
SELECT DISTINCT
    first_name,
    last_name
FROM employees;
```

---

# 12. UPDATE

Update one employee:

```sql
UPDATE employees
SET salary = 2000
WHERE employee_id = 1;
```

Update multiple columns:

```sql
UPDATE employees
SET
    salary = 2200,
    email = 'newemail@example.com'
WHERE employee_id = 1;
```

Always use `WHERE` when you don't want to update every row.

```sql
COMMIT;
```

---

# 13. DELETE

Delete one row:

```sql
DELETE FROM employees
WHERE employee_id = 3;
```

Delete all rows:

```sql
DELETE FROM employees;
```

> Be careful: without `WHERE`, every row is deleted.

---

# 14. Operators

## Comparison

```sql
=
<>
!=
>
<
>=
<=
```

Example:

```sql
SELECT *
FROM employees
WHERE salary >= 1500;
```

## BETWEEN

```sql
SELECT *
FROM employees
WHERE salary BETWEEN 1000 AND 2000;
```

## IN

```sql
SELECT *
FROM employees
WHERE employee_id IN (1, 2, 3);
```

## LIKE

```sql
SELECT *
FROM employees
WHERE first_name LIKE 'D%';
```

Meaning:

```text
D%     starts with D
%D     ends with D
%D%    contains D
```

---

# 15. Aggregate Functions

Common functions:

```text
COUNT
SUM
AVG
MIN
MAX
```

Count:

```sql
SELECT COUNT(*) AS total_employees
FROM employees;
```

Sum:

```sql
SELECT SUM(salary) AS total_salary
FROM employees;
```

Average:

```sql
SELECT AVG(salary) AS average_salary
FROM employees;
```

Minimum:

```sql
SELECT MIN(salary) AS minimum_salary
FROM employees;
```

Maximum:

```sql
SELECT MAX(salary) AS maximum_salary
FROM employees;
```

---

# 16. GROUP BY

Example:

```sql
SELECT
    hire_date,
    COUNT(*) AS employee_count
FROM employees
GROUP BY hire_date;
```

Another example:

```sql
SELECT
    first_name,
    COUNT(*) AS total
FROM employees
GROUP BY first_name;
```

### Khmer

`GROUP BY` ប្រើសម្រាប់បែងចែក data ជាក្រុម ហើយប្រើជាមួយ aggregate functions។

---

# 17. HAVING

`HAVING` filters groups.

```sql
SELECT
    first_name,
    COUNT(*) AS total
FROM employees
GROUP BY first_name
HAVING COUNT(*) > 1;
```

Difference:

```text
WHERE  → filters rows
HAVING → filters groups
```

---

# 18. String Functions

## UPPER

```sql
SELECT UPPER(first_name)
FROM employees;
```

## LOWER

```sql
SELECT LOWER(first_name)
FROM employees;
```

## INITCAP

```sql
SELECT INITCAP(first_name)
FROM employees;
```

## LENGTH

```sql
SELECT
    first_name,
    LENGTH(first_name) AS name_length
FROM employees;
```

## SUBSTR

```sql
SELECT SUBSTR(first_name, 1, 3)
FROM employees;
```

## CONCAT

```sql
SELECT CONCAT(first_name, last_name)
FROM employees;
```

Using `||`:

```sql
SELECT
    first_name || ' ' || last_name AS full_name
FROM employees;
```

---

# 19. Number Functions

## ROUND

```sql
SELECT ROUND(123.456, 2)
FROM dual;
```

Result:

```text
123.46
```

## CEIL

```sql
SELECT CEIL(10.2)
FROM dual;
```

## FLOOR

```sql
SELECT FLOOR(10.9)
FROM dual;
```

## MOD

```sql
SELECT MOD(10, 3)
FROM dual;
```

---

# 20. Date Functions

Current date:

```sql
SELECT SYSDATE
FROM dual;
```

Current timestamp:

```sql
SELECT SYSTIMESTAMP
FROM dual;
```

Add months:

```sql
SELECT ADD_MONTHS(SYSDATE, 3)
FROM dual;
```

Last day:

```sql
SELECT LAST_DAY(SYSDATE)
FROM dual;
```

Extract year:

```sql
SELECT EXTRACT(YEAR FROM SYSDATE)
FROM dual;
```

---

# 21. CASE

Simple CASE:

```sql
SELECT
    first_name,
    salary,
    CASE
        WHEN salary >= 2000 THEN 'HIGH'
        WHEN salary >= 1500 THEN 'MEDIUM'
        ELSE 'LOW'
    END AS salary_level
FROM employees;
```

### Khmer

`CASE` គឺដូចជា `if / else` ក្នុង SQL។

---

# 22. JOINS

Create departments:

```sql
CREATE TABLE departments (
    department_id NUMBER PRIMARY KEY,
    department_name VARCHAR2(100) NOT NULL
);
```

Insert data:

```sql
INSERT INTO departments (
    department_id,
    department_name
)
VALUES (
    10,
    'IT'
);

INSERT INTO departments (
    department_id,
    department_name
)
VALUES (
    20,
    'Finance'
);

COMMIT;
```

Add department to employees:

```sql
ALTER TABLE employees
ADD department_id NUMBER;
```

Update:

```sql
UPDATE employees
SET department_id = 10
WHERE employee_id IN (1, 2);

UPDATE employees
SET department_id = 20
WHERE employee_id = 3;

COMMIT;
```

## INNER JOIN

```sql
SELECT
    e.employee_id,
    e.first_name,
    d.department_name
FROM employees e
INNER JOIN departments d
    ON e.department_id = d.department_id;
```

## LEFT JOIN

```sql
SELECT
    e.employee_id,
    e.first_name,
    d.department_name
FROM employees e
LEFT JOIN departments d
    ON e.department_id = d.department_id;
```

## RIGHT JOIN

```sql
SELECT
    e.employee_id,
    e.first_name,
    d.department_name
FROM employees e
RIGHT JOIN departments d
    ON e.department_id = d.department_id;
```

## FULL OUTER JOIN

```sql
SELECT
    e.employee_id,
    e.first_name,
    d.department_name
FROM employees e
FULL OUTER JOIN departments d
    ON e.department_id = d.department_id;
```

### Khmer

Join គឺប្រើសម្រាប់ភ្ជាប់ Table ពីរ ឬច្រើនតាម column ដែលមានទំនាក់ទំនងគ្នា។

---

# 23. Subqueries

Find employees earning more than average salary:

```sql
SELECT
    employee_id,
    first_name,
    salary
FROM employees
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
);
```

Subquery with IN:

```sql
SELECT *
FROM employees
WHERE department_id IN (
    SELECT department_id
    FROM departments
    WHERE department_name = 'IT'
);
```

---

# 24. Common Table Expressions

Use `WITH`:

```sql
WITH employee_salary AS (
    SELECT
        employee_id,
        first_name,
        salary
    FROM employees
    WHERE salary > 1000
)
SELECT *
FROM employee_salary;
```

Multiple CTEs:

```sql
WITH
employee_data AS (
    SELECT *
    FROM employees
),
department_data AS (
    SELECT *
    FROM departments
)
SELECT
    e.first_name,
    d.department_name
FROM employee_data e
JOIN department_data d
    ON e.department_id = d.department_id;
```

### Khmer

CTE (`WITH`) ធ្វើឱ្យ query ធំៗ អានងាយ និងរៀបចំបានល្អ។

---

# 25. Constraints

Important constraints:

```text
PRIMARY KEY
FOREIGN KEY
UNIQUE
NOT NULL
CHECK
```

Example:

```sql
CREATE TABLE users (
    user_id NUMBER
        CONSTRAINT pk_users PRIMARY KEY,

    username VARCHAR2(50)
        CONSTRAINT uq_users_username UNIQUE
        CONSTRAINT nn_users_username NOT NULL,

    age NUMBER
        CONSTRAINT chk_users_age CHECK (age >= 18)
);
```

Foreign key:

```sql
CREATE TABLE orders (
    order_id NUMBER
        CONSTRAINT pk_orders PRIMARY KEY,

    user_id NUMBER
        CONSTRAINT fk_orders_user
        REFERENCES users(user_id),

    order_date DATE DEFAULT SYSDATE
);
```

---

# 26. ALTER TABLE

Add column:

```sql
ALTER TABLE employees
ADD phone VARCHAR2(30);
```

Modify column:

```sql
ALTER TABLE employees
MODIFY phone VARCHAR2(50);
```

Rename column:

```sql
ALTER TABLE employees
RENAME COLUMN phone TO phone_number;
```

Rename table:

```sql
RENAME employees TO staff;
```

Drop column:

```sql
ALTER TABLE staff
DROP COLUMN phone_number;
```

---

# 27. Views

Create view:

```sql
CREATE OR REPLACE VIEW employee_details AS
SELECT
    e.employee_id,
    e.first_name,
    e.last_name,
    e.salary,
    d.department_name
FROM employees e
LEFT JOIN departments d
    ON e.department_id = d.department_id;
```

Use:

```sql
SELECT *
FROM employee_details;
```

Drop:

```sql
DROP VIEW employee_details;
```

### Khmer

View គឺជា virtual table ដែលបង្កើតពី SQL query។

---

# 28. Sequences

Create:

```sql
CREATE SEQUENCE employee_seq
START WITH 100
INCREMENT BY 1
NOCACHE;
```

Get next value:

```sql
SELECT employee_seq.NEXTVAL
FROM dual;
```

Get current value after `NEXTVAL` has been referenced in the session:

```sql
SELECT employee_seq.CURRVAL
FROM dual;
```

Use with INSERT:

```sql
INSERT INTO employees (
    employee_id,
    first_name,
    last_name,
    salary
)
VALUES (
    employee_seq.NEXTVAL,
    'Dara',
    'Sok',
    1500
);
```

---

# 29. Identity Columns

Modern Oracle can generate IDs automatically.

```sql
CREATE TABLE customers (
    customer_id NUMBER
        GENERATED BY DEFAULT AS IDENTITY
        PRIMARY KEY,

    customer_name VARCHAR2(100) NOT NULL
);
```

Insert:

```sql
INSERT INTO customers (customer_name)
VALUES ('Dara');
```

Check:

```sql
SELECT *
FROM customers;
```

---

# 30. Indexes

Create index:

```sql
CREATE INDEX idx_employee_email
ON employees(email);
```

Composite index:

```sql
CREATE INDEX idx_employee_department_salary
ON employees(department_id, salary);
```

Unique index:

```sql
CREATE UNIQUE INDEX idx_employee_unique_email
ON employees(email);
```

Drop:

```sql
DROP INDEX idx_employee_email;
```

### Important

Indexes can improve reads, but too many indexes can increase the cost of:

```text
INSERT
UPDATE
DELETE
```

---

# 31. Transactions

A transaction contains one or more database operations.

```sql
UPDATE employees
SET salary = salary + 100
WHERE department_id = 10;

COMMIT;
```

Rollback:

```sql
UPDATE employees
SET salary = salary + 1000
WHERE employee_id = 1;

ROLLBACK;
```

Savepoint:

```sql
UPDATE employees
SET salary = salary + 100
WHERE employee_id = 1;

SAVEPOINT salary_update;

UPDATE employees
SET salary = salary + 200
WHERE employee_id = 2;

ROLLBACK TO salary_update;

COMMIT;
```

### Khmer

* `COMMIT` = រក្សាទុកការផ្លាស់ប្តូរ
* `ROLLBACK` = បោះបង់ការផ្លាស់ប្តូរ
* `SAVEPOINT` = បង្កើតចំណុចដែលអាច rollback ត្រឡប់ទៅ

---

# 32. MERGE

`MERGE` is useful for UPSERT-style operations.

```sql
MERGE INTO customers c
USING (
    SELECT
        1 AS customer_id,
        'Dara Updated' AS customer_name
    FROM dual
) source
ON (c.customer_id = source.customer_id)

WHEN MATCHED THEN
    UPDATE SET
        c.customer_name = source.customer_name

WHEN NOT MATCHED THEN
    INSERT (
        customer_id,
        customer_name
    )
    VALUES (
        source.customer_id,
        source.customer_name
    );
```

---

# 33. Set Operators

## UNION

```sql
SELECT first_name
FROM employees
UNION
SELECT customer_name
FROM customers;
```

`UNION` removes duplicates.

## UNION ALL

```sql
SELECT first_name
FROM employees
UNION ALL
SELECT customer_name
FROM customers;
```

Keeps duplicates.

## INTERSECT

```sql
SELECT department_id
FROM employees
INTERSECT
SELECT department_id
FROM departments;
```

## MINUS

```sql
SELECT department_id
FROM departments
MINUS
SELECT department_id
FROM employees;
```

---

# 34. NULL Handling

Check NULL:

```sql
SELECT *
FROM employees
WHERE department_id IS NULL;
```

Not NULL:

```sql
SELECT *
FROM employees
WHERE department_id IS NOT NULL;
```

Use `NVL`:

```sql
SELECT
    first_name,
    NVL(salary, 0) AS salary
FROM employees;
```

Use `COALESCE`:

```sql
SELECT
    COALESCE(email, 'No Email') AS email
FROM employees;
```

---

# 35. Oracle ROWID and ROWNUM

ROWID:

```sql
SELECT
    ROWID,
    employee_id,
    first_name
FROM employees;
```

`ROWNUM`:

```sql
SELECT *
FROM employees
WHERE ROWNUM <= 5;
```

### Important

`ROWNUM` is assigned during query processing. For modern pagination, prefer `OFFSET ... FETCH` or `ROW_NUMBER()` when appropriate.

---

# 36. Pagination

Oracle supports:

```sql
SELECT
    employee_id,
    first_name,
    salary
FROM employees
ORDER BY employee_id
OFFSET 0 ROWS
FETCH NEXT 10 ROWS ONLY;
```

Page 2:

```sql
SELECT
    employee_id,
    first_name,
    salary
FROM employees
ORDER BY employee_id
OFFSET 10 ROWS
FETCH NEXT 10 ROWS ONLY;
```

---

# 37. Analytic Functions

Analytic functions calculate values across related rows without collapsing them into one row.

## ROW_NUMBER

```sql
SELECT
    employee_id,
    first_name,
    salary,
    ROW_NUMBER() OVER (
        ORDER BY salary DESC
    ) AS row_number
FROM employees;
```

## RANK

```sql
SELECT
    employee_id,
    first_name,
    salary,
    RANK() OVER (
        ORDER BY salary DESC
    ) AS salary_rank
FROM employees;
```

## DENSE_RANK

```sql
SELECT
    employee_id,
    first_name,
    salary,
    DENSE_RANK() OVER (
        ORDER BY salary DESC
    ) AS salary_rank
FROM employees;
```

---

# 38. Window Functions

Partition by department:

```sql
SELECT
    employee_id,
    first_name,
    department_id,
    salary,

    AVG(salary) OVER (
        PARTITION BY department_id
    ) AS department_average

FROM employees;
```

Running total:

```sql
SELECT
    employee_id,
    salary,

    SUM(salary) OVER (
        ORDER BY employee_id
        ROWS BETWEEN UNBOUNDED PRECEDING
        AND CURRENT ROW
    ) AS running_total

FROM employees;
```

Previous row:

```sql
SELECT
    employee_id,
    salary,

    LAG(salary) OVER (
        ORDER BY employee_id
    ) AS previous_salary

FROM employees;
```

Next row:

```sql
SELECT
    employee_id,
    salary,

    LEAD(salary) OVER (
        ORDER BY employee_id
    ) AS next_salary

FROM employees;
```

---

# 39. PL/SQL

PL/SQL means:

**Procedural Language/SQL**

It allows you to write procedural programs inside Oracle.

Basic structure:

```sql
BEGIN
    DBMS_OUTPUT.PUT_LINE('Hello Oracle');
END;
/
```

Enable output in SQL*Plus/SQLcl:

```sql
SET SERVEROUTPUT ON;
```

---

# 40. Variables

```sql
DECLARE
    v_name VARCHAR2(100);
    v_salary NUMBER;
BEGIN
    v_name := 'Dara';
    v_salary := 1500;

    DBMS_OUTPUT.PUT_LINE(
        'Name: ' || v_name
    );

    DBMS_OUTPUT.PUT_LINE(
        'Salary: ' || v_salary
    );
END;
/
```

Using `%TYPE`:

```sql
DECLARE
    v_salary employees.salary%TYPE;
BEGIN
    SELECT salary
    INTO v_salary
    FROM employees
    WHERE employee_id = 1;

    DBMS_OUTPUT.PUT_LINE(
        'Salary: ' || v_salary
    );
END;
/
```

---

# 41. IF / ELSIF / ELSE

```sql
DECLARE
    v_salary NUMBER := 1800;
BEGIN
    IF v_salary >= 2000 THEN
        DBMS_OUTPUT.PUT_LINE('High salary');

    ELSIF v_salary >= 1500 THEN
        DBMS_OUTPUT.PUT_LINE('Medium salary');

    ELSE
        DBMS_OUTPUT.PUT_LINE('Low salary');
    END IF;
END;
/
```

---

# 42. Loops

## Basic LOOP

```sql
DECLARE
    v_counter NUMBER := 1;
BEGIN
    LOOP
        DBMS_OUTPUT.PUT_LINE(v_counter);

        v_counter := v_counter + 1;

        EXIT WHEN v_counter > 5;
    END LOOP;
END;
/
```

## WHILE LOOP

```sql
DECLARE
    v_counter NUMBER := 1;
BEGIN
    WHILE v_counter <= 5 LOOP

        DBMS_OUTPUT.PUT_LINE(v_counter);

        v_counter := v_counter + 1;

    END LOOP;
END;
/
```

## FOR LOOP

```sql
BEGIN
    FOR i IN 1..5 LOOP
        DBMS_OUTPUT.PUT_LINE(i);
    END LOOP;
END;
/
```

---

# 43. Cursors

## Explicit Cursor

```sql
DECLARE

    CURSOR employee_cursor IS
        SELECT
            employee_id,
            first_name,
            salary
        FROM employees;

BEGIN

    FOR employee_record IN employee_cursor LOOP

        DBMS_OUTPUT.PUT_LINE(
            employee_record.employee_id
            || ' - '
            || employee_record.first_name
            || ' - '
            || employee_record.salary
        );

    END LOOP;

END;
/
```

### Cursor with parameter

```sql
DECLARE

    CURSOR employee_cursor (
        p_department_id NUMBER
    ) IS
        SELECT
            employee_id,
            first_name,
            salary
        FROM employees
        WHERE department_id = p_department_id;

BEGIN

    FOR employee_record IN employee_cursor(10) LOOP

        DBMS_OUTPUT.PUT_LINE(
            employee_record.first_name
        );

    END LOOP;

END;
/
```

---

# 44. Exceptions

Basic exception handling:

```sql
DECLARE
    v_salary employees.salary%TYPE;
BEGIN

    SELECT salary
    INTO v_salary
    FROM employees
    WHERE employee_id = 999999;

    DBMS_OUTPUT.PUT_LINE(v_salary);

EXCEPTION

    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE(
            'Employee not found'
        );

    WHEN TOO_MANY_ROWS THEN
        DBMS_OUTPUT.PUT_LINE(
            'More than one employee found'
        );

    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE(
            'Error: ' || SQLERRM
        );

END;
/
```

### Important Exceptions

```text
NO_DATA_FOUND
TOO_MANY_ROWS
DUP_VAL_ON_INDEX
VALUE_ERROR
ZERO_DIVIDE
OTHERS
```

---

# 45. Procedures

Create procedure:

```sql
CREATE OR REPLACE PROCEDURE increase_salary (
    p_employee_id IN employees.employee_id%TYPE,
    p_amount      IN NUMBER
)
AS
BEGIN

    UPDATE employees
    SET salary = salary + p_amount
    WHERE employee_id = p_employee_id;

END increase_salary;
/
```

Execute:

```sql
BEGIN
    increase_salary(1, 100);
    COMMIT;
END;
/
```

---

# 46. Functions

Create function:

```sql
CREATE OR REPLACE FUNCTION get_employee_salary (
    p_employee_id IN employees.employee_id%TYPE
)
RETURN NUMBER
AS
    v_salary employees.salary%TYPE;
BEGIN

    SELECT salary
    INTO v_salary
    FROM employees
    WHERE employee_id = p_employee_id;

    RETURN v_salary;

EXCEPTION
    WHEN NO_DATA_FOUND THEN
        RETURN NULL;
END get_employee_salary;
/
```

Use:

```sql
SELECT get_employee_salary(1)
FROM dual;
```

### Difference

```text
Procedure → performs an operation
Function  → returns a value
```

---

# 47. Parameters

PL/SQL supports:

```text
IN
OUT
IN OUT
```

Example:

```sql
CREATE OR REPLACE PROCEDURE get_employee_info (
    p_employee_id IN employees.employee_id%TYPE,
    p_name OUT VARCHAR2,
    p_salary OUT NUMBER
)
AS
BEGIN

    SELECT
        first_name,
        salary
    INTO
        p_name,
        p_salary
    FROM employees
    WHERE employee_id = p_employee_id;

END;
/
```

Call:

```sql
DECLARE
    v_name VARCHAR2(100);
    v_salary NUMBER;
BEGIN

    get_employee_info(
        1,
        v_name,
        v_salary
    );

    DBMS_OUTPUT.PUT_LINE(
        'Name: ' || v_name
    );

    DBMS_OUTPUT.PUT_LINE(
        'Salary: ' || v_salary
    );

END;
/
```

---

# 48. Triggers

A trigger automatically executes when a specified database event occurs.

Create audit table:

```sql
CREATE TABLE employee_audit (
    audit_id NUMBER GENERATED BY DEFAULT AS IDENTITY,
    employee_id NUMBER,
    action_type VARCHAR2(20),
    action_date TIMESTAMP DEFAULT SYSTIMESTAMP
);
```

Create trigger:

```sql
CREATE OR REPLACE TRIGGER trg_employee_delete
AFTER DELETE ON employees
FOR EACH ROW
BEGIN

    INSERT INTO employee_audit (
        employee_id,
        action_type
    )
    VALUES (
        :OLD.employee_id,
        'DELETE'
    );

END;
/
```

Delete:

```sql
DELETE FROM employees
WHERE employee_id = 1;

COMMIT;
```

Check audit:

```sql
SELECT *
FROM employee_audit;
```

### `:OLD` and `:NEW`

For row-level triggers:

```text
:OLD → old value
:NEW → new value
```

---

# 49. Packages

Packages group related PL/SQL objects.

## Package Specification

```sql
CREATE OR REPLACE PACKAGE employee_pkg AS

    PROCEDURE increase_salary (
        p_employee_id IN NUMBER,
        p_amount IN NUMBER
    );

    FUNCTION get_salary (
        p_employee_id IN NUMBER
    )
    RETURN NUMBER;

END employee_pkg;
/
```

## Package Body

```sql
CREATE OR REPLACE PACKAGE BODY employee_pkg AS

    PROCEDURE increase_salary (
        p_employee_id IN NUMBER,
        p_amount IN NUMBER
    )
    AS
    BEGIN

        UPDATE employees
        SET salary = salary + p_amount
        WHERE employee_id = p_employee_id;

    END increase_salary;


    FUNCTION get_salary (
        p_employee_id IN NUMBER
    )
    RETURN NUMBER
    AS
        v_salary NUMBER;
    BEGIN

        SELECT salary
        INTO v_salary
        FROM employees
        WHERE employee_id = p_employee_id;

        RETURN v_salary;

    END get_salary;

END employee_pkg;
/
```

Use package:

```sql
BEGIN
    employee_pkg.increase_salary(1, 100);
    COMMIT;
END;
/
```

Function:

```sql
SELECT employee_pkg.get_salary(1)
FROM dual;
```

---

# 50. Dynamic SQL

Oracle supports dynamic SQL with `EXECUTE IMMEDIATE`.

Example:

```sql
DECLARE
    v_sql VARCHAR2(1000);
BEGIN

    v_sql := 'UPDATE employees
              SET salary = salary + :amount
              WHERE employee_id = :id';

    EXECUTE IMMEDIATE v_sql
        USING 100, 1;

END;
/
```

Commit:

```sql
COMMIT;
```

### Important

Avoid concatenating untrusted user input directly into SQL.

Bad:

```sql
v_sql := 'SELECT * FROM users WHERE username = '''
         || p_username
         || '''';
```

Prefer bind variables.

---

# 51. Bulk Processing

Bulk operations can reduce PL/SQL-to-SQL context switching.

## BULK COLLECT

```sql
DECLARE

    TYPE employee_table IS TABLE OF employees%ROWTYPE;

    v_employees employee_table;

BEGIN

    SELECT *
    BULK COLLECT INTO v_employees
    FROM employees;

    FOR i IN 1..v_employees.COUNT LOOP

        DBMS_OUTPUT.PUT_LINE(
            v_employees(i).first_name
        );

    END LOOP;

END;
/
```

## FORALL

```sql
DECLARE

    TYPE employee_id_table IS
        TABLE OF employees.employee_id%TYPE;

    v_ids employee_id_table :=
        employee_id_table(1, 2, 3);

BEGIN

    FORALL i IN 1..v_ids.COUNT

        UPDATE employees
        SET salary = salary + 100
        WHERE employee_id = v_ids(i);

    COMMIT;

END;
/
```

---

# 52. JSON

Oracle supports JSON data.

Create table:

```sql
CREATE TABLE json_users (
    id NUMBER GENERATED BY DEFAULT AS IDENTITY,
    data JSON
);
```

Insert JSON:

```sql
INSERT INTO json_users (data)
VALUES (
    JSON_OBJECT(
        'name' VALUE 'Dara',
        'age' VALUE 25,
        'city' VALUE 'Phnom Penh'
    )
);
```

Query:

```sql
SELECT
    data.name,
    data.age,
    data.city
FROM json_users;
```

Using `JSON_VALUE`:

```sql
SELECT
    JSON_VALUE(
        data,
        '$.name'
    ) AS name
FROM json_users;
```

---

# 53. Database Design

A good database design starts with entities.

Example:

```text
Users
  │
  ├── Orders
  │     │
  │     └── Order Items
  │
  └── Addresses
```

Possible tables:

```text
users
products
orders
order_items
categories
payments
addresses
```

---

# 54. Normalization

Common normal forms:

```text
1NF
2NF
3NF
BCNF
```

## Bad Design

```text
orders

order_id
customer_name
customer_phone
product_1
product_2
product_3
```

## Better Design

```text
customers
---------
customer_id
name
phone

orders
------
order_id
customer_id

order_items
-----------
order_item_id
order_id
product_id
quantity
```

### Khmer

Normalization គឺជាវិធីរៀបចំ Database ដើម្បី:

* កាត់បន្ថយ duplicate data
* កាត់បន្ថយ inconsistency
* ធ្វើឱ្យ database មាន structure ល្អ

---

# 55. Security

Create user:

```sql
CREATE USER app_user
IDENTIFIED BY "StrongPassword123";
```

Grant connection privilege:

```sql
GRANT CREATE SESSION
TO app_user;
```

Grant table privileges:

```sql
GRANT SELECT, INSERT, UPDATE
ON employees
TO app_user;
```

Revoke:

```sql
REVOKE UPDATE
ON employees
FROM app_user;
```

### Security Best Practices

Do not:

```text
Store plaintext passwords
Hard-code database passwords
Give unnecessary privileges
Use dynamic SQL with untrusted input
Use SYS for application operations
```

Use:

```text
Least privilege
Bind variables
Roles
Password hashing
Secrets management
Auditing
```

---

# 56. Performance

## Use EXPLAIN PLAN

```sql
EXPLAIN PLAN FOR
SELECT *
FROM employees
WHERE department_id = 10;
```

Display:

```sql
SELECT *
FROM TABLE(DBMS_XPLAN.DISPLAY);
```

## Index Example

```sql
CREATE INDEX idx_employees_department
ON employees(department_id);
```

## Avoid

```sql
SELECT *
FROM employees;
```

when you only need:

```sql
SELECT
    employee_id,
    first_name,
    salary
FROM employees;
```

### Performance Checklist

```text
✔ Use appropriate indexes
✔ Select only required columns
✔ Use bind variables
✔ Analyze execution plans
✔ Avoid unnecessary joins
✔ Avoid unnecessary functions on indexed columns
✔ Use pagination for large result sets
✔ Use bulk operations for large PL/SQL processing
✔ Keep transactions appropriately sized
```

---

# 57. Useful Oracle Commands

## List Tables

```sql
SELECT table_name
FROM user_tables
ORDER BY table_name;
```

## List Views

```sql
SELECT view_name
FROM user_views
ORDER BY view_name;
```

## List Sequences

```sql
SELECT sequence_name
FROM user_sequences
ORDER BY sequence_name;
```

## List Procedures

```sql
SELECT object_name
FROM user_objects
WHERE object_type = 'PROCEDURE'
ORDER BY object_name;
```

## List Functions

```sql
SELECT object_name
FROM user_objects
WHERE object_type = 'FUNCTION'
ORDER BY object_name;
```

## List Packages

```sql
SELECT object_name
FROM user_objects
WHERE object_type = 'PACKAGE'
ORDER BY object_name;
```

## List Triggers

```sql
SELECT trigger_name
FROM user_triggers
ORDER BY trigger_name;
```

## List All Objects

```sql
SELECT
    object_name,
    object_type,
    status
FROM user_objects
ORDER BY object_type, object_name;
```

---

# 58. Complete Mini Project

Let's build a small **Oracle E-Commerce Database**.

## Step 1 — Categories

```sql
CREATE TABLE categories (
    category_id NUMBER
        GENERATED BY DEFAULT AS IDENTITY
        CONSTRAINT pk_categories PRIMARY KEY,

    category_name VARCHAR2(100)
        CONSTRAINT uq_categories_name UNIQUE
        CONSTRAINT nn_categories_name NOT NULL
);
```

---

## Step 2 — Products

```sql
CREATE TABLE products (
    product_id NUMBER
        GENERATED BY DEFAULT AS IDENTITY
        CONSTRAINT pk_products PRIMARY KEY,

    category_id NUMBER
        CONSTRAINT fk_products_category
        REFERENCES categories(category_id),

    product_name VARCHAR2(200)
        CONSTRAINT nn_products_name NOT NULL,

    price NUMBER(12, 2)
        CONSTRAINT chk_products_price CHECK (price >= 0),

    stock_quantity NUMBER
        CONSTRAINT chk_products_stock CHECK (stock_quantity >= 0),

    created_at TIMESTAMP DEFAULT SYSTIMESTAMP
);
```

---

## Step 3 — Customers

```sql
CREATE TABLE customers (
    customer_id NUMBER
        GENERATED BY DEFAULT AS IDENTITY
        CONSTRAINT pk_customers PRIMARY KEY,

    customer_name VARCHAR2(150)
        CONSTRAINT nn_customers_name NOT NULL,

    email VARCHAR2(200)
        CONSTRAINT uq_customers_email UNIQUE,

    created_at TIMESTAMP DEFAULT SYSTIMESTAMP
);
```

---

## Step 4 — Orders

```sql
CREATE TABLE orders (
    order_id NUMBER
        GENERATED BY DEFAULT AS IDENTITY
        CONSTRAINT pk_orders PRIMARY KEY,

    customer_id NUMBER
        CONSTRAINT fk_orders_customer
        REFERENCES customers(customer_id),

    order_date TIMESTAMP DEFAULT SYSTIMESTAMP,

    status VARCHAR2(30) DEFAULT 'PENDING'
        CONSTRAINT chk_orders_status
        CHECK (
            status IN (
                'PENDING',
                'PAID',
                'SHIPPED',
                'COMPLETED',
                'CANCELLED'
            )
        )
);
```

---

## Step 5 — Order Items

```sql
CREATE TABLE order_items (
    order_item_id NUMBER
        GENERATED BY DEFAULT AS IDENTITY
        CONSTRAINT pk_order_items PRIMARY KEY,

    order_id NUMBER
        CONSTRAINT fk_order_items_order
        REFERENCES orders(order_id),

    product_id NUMBER
        CONSTRAINT fk_order_items_product
        REFERENCES products(product_id),

    quantity NUMBER
        CONSTRAINT chk_order_items_quantity
        CHECK (quantity > 0),

    unit_price NUMBER(12, 2)
        CONSTRAINT chk_order_items_price
        CHECK (unit_price >= 0)
);
```

---

## Step 6 — Insert Categories

```sql
INSERT INTO categories (category_name)
VALUES ('Electronics');

INSERT INTO categories (category_name)
VALUES ('Computers');

INSERT INTO categories (category_name)
VALUES ('Accessories');

COMMIT;
```

---

## Step 7 — Insert Products

```sql
INSERT INTO products (
    category_id,
    product_name,
    price,
    stock_quantity
)
VALUES (
    1,
    'Keyboard',
    35.00,
    100
);

INSERT INTO products (
    category_id,
    product_name,
    price,
    stock_quantity
)
VALUES (
    1,
    'Mouse',
    20.00,
    150
);

INSERT INTO products (
    category_id,
    product_name,
    price,
    stock_quantity
)
VALUES (
    2,
    'Laptop',
    900.00,
    20
);

COMMIT;
```

---

## Step 8 — Insert Customer

```sql
INSERT INTO customers (
    customer_name,
    email
)
VALUES (
    'Dara Sok',
    'dara@example.com'
);

COMMIT;
```

---

## Step 9 — Create Order

```sql
INSERT INTO orders (
    customer_id,
    status
)
VALUES (
    1,
    'PENDING'
);

COMMIT;
```

---

## Step 10 — Add Order Items

```sql
INSERT INTO order_items (
    order_id,
    product_id,
    quantity,
    unit_price
)
SELECT
    1,
    product_id,
    2,
    price
FROM products
WHERE product_name = 'Keyboard';
```

Add another:

```sql
INSERT INTO order_items (
    order_id,
    product_id,
    quantity,
    unit_price
)
SELECT
    1,
    product_id,
    1,
    price
FROM products
WHERE product_name = 'Mouse';

COMMIT;
```

---

## Step 11 — Calculate Order Total

```sql
SELECT
    oi.order_id,
    SUM(
        oi.quantity * oi.unit_price
    ) AS order_total
FROM order_items oi
WHERE oi.order_id = 1
GROUP BY oi.order_id;
```

---

## Step 12 — Complete Order Query

```sql
SELECT
    o.order_id,
    c.customer_name,
    o.order_date,
    o.status,

    SUM(
        oi.quantity * oi.unit_price
    ) AS total_amount

FROM orders o

JOIN customers c
    ON c.customer_id = o.customer_id

JOIN order_items oi
    ON oi.order_id = o.order_id

GROUP BY
    o.order_id,
    c.customer_name,
    o.order_date,
    o.status

ORDER BY o.order_id;
```

---

# 59. Best Practices

## Naming

Use clear names:

```text
employee_id
department_id
created_at
updated_at
order_total
```

Avoid:

```text
x
a
abc
data1
temp2
```

---

## SQL Formatting

Bad:

```sql
SELECT e.employee_id,e.first_name,d.department_name FROM employees e JOIN departments d ON e.department_id=d.department_id;
```

Good:

```sql
SELECT
    e.employee_id,
    e.first_name,
    d.department_name
FROM employees e
JOIN departments d
    ON e.department_id = d.department_id;
```

---

## Use Explicit Columns

Prefer:

```sql
SELECT
    employee_id,
    first_name,
    salary
FROM employees;
```

instead of:

```sql
SELECT *
FROM employees;
```

---

## Use Bind Variables

Example:

```sql
SELECT *
FROM employees
WHERE employee_id = :employee_id;
```

This is safer and generally better for reusable SQL.

---

## Use Constraints

Do not depend entirely on application code.

Database:

```sql
CONSTRAINT pk_users PRIMARY KEY
```

Application:

```text
Validation
```

Both should work together.

---

# 60. Learning Roadmap

## 🟢 Beginner

Learn these first:

```text
1. Oracle Database basics
2. Tables
3. Data types
4. CREATE TABLE
5. INSERT
6. SELECT
7. WHERE
8. ORDER BY
9. UPDATE
10. DELETE
11. NULL
12. Functions
13. GROUP BY
14. HAVING
```

---

## 🟡 Intermediate

Then learn:

```text
1. INNER JOIN
2. LEFT JOIN
3. RIGHT JOIN
4. FULL JOIN
5. Subqueries
6. CTE
7. Constraints
8. Views
9. Sequences
10. Identity columns
11. Indexes
12. Transactions
13. MERGE
14. Pagination
15. Set operators
```

---

## 🟠 Advanced SQL

Learn:

```text
1. Analytic functions
2. Window functions
3. ROW_NUMBER
4. RANK
5. DENSE_RANK
6. LAG
7. LEAD
8. CTE
9. Recursive queries
10. Query optimization
11. Execution plans
12. Index optimization
13. Advanced aggregation
```

---

## 🔴 Advanced PL/SQL

Learn:

```text
1. Variables
2. Conditions
3. Loops
4. Cursors
5. Exceptions
6. Procedures
7. Functions
8. IN / OUT / IN OUT
9. Triggers
10. Packages
11. Dynamic SQL
12. BULK COLLECT
13. FORALL
14. Advanced exception handling
```

---

## 🟣 Professional Oracle Development

After mastering SQL and PL/SQL:

```text
Database Architecture
        ↓
Data Modeling
        ↓
Normalization
        ↓
SQL
        ↓
PL/SQL
        ↓
Indexes
        ↓
Transactions
        ↓
Query Optimization
        ↓
Security
        ↓
Backup & Recovery
        ↓
High Availability
        ↓
Production Database Administration
```

---

# 🎯 SQL Cheat Sheet

## Create

```sql
CREATE TABLE table_name (
    id NUMBER PRIMARY KEY,
    name VARCHAR2(100)
);
```

## Insert

```sql
INSERT INTO table_name (
    id,
    name
)
VALUES (
    1,
    'Dara'
);
```

## Select

```sql
SELECT *
FROM table_name;
```

## Update

```sql
UPDATE table_name
SET name = 'Sok'
WHERE id = 1;
```

## Delete

```sql
DELETE FROM table_name
WHERE id = 1;
```

## Join

```sql
SELECT *
FROM table_a a
JOIN table_b b
    ON a.id = b.a_id;
```

## Group

```sql
SELECT
    department_id,
    COUNT(*) AS total
FROM employees
GROUP BY department_id;
```

## Transaction

```sql
COMMIT;
```

```sql
ROLLBACK;
```

---

# 🎯 PL/SQL Cheat Sheet

## Anonymous Block

```sql
BEGIN
    DBMS_OUTPUT.PUT_LINE('Hello');
END;
/
```

## Variable

```sql
DECLARE
    v_name VARCHAR2(100);
BEGIN
    v_name := 'Dara';

    DBMS_OUTPUT.PUT_LINE(v_name);
END;
/
```

## Condition

```sql
IF condition THEN
    -- code
ELSIF condition THEN
    -- code
ELSE
    -- code
END IF;
```

## Loop

```sql
FOR i IN 1..10 LOOP
    DBMS_OUTPUT.PUT_LINE(i);
END LOOP;
```

## Procedure

```sql
CREATE OR REPLACE PROCEDURE hello
AS
BEGIN
    DBMS_OUTPUT.PUT_LINE('Hello');
END;
/
```

## Function

```sql
CREATE OR REPLACE FUNCTION add_numbers (
    a NUMBER,
    b NUMBER
)
RETURN NUMBER
AS
BEGIN
    RETURN a + b;
END;
/
```

## Exception

```sql
EXCEPTION
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE(SQLERRM);
```

---

# 🧠 English + Khmer Summary

| Topic       | English                          | Khmer                             |
| ----------- | -------------------------------- | --------------------------------- |
| Database    | Stores and manages data          | រក្សាទុក និងគ្រប់គ្រងទិន្នន័យ     |
| Table       | Stores rows and columns          | ផ្ទុកជួរដេក និងជួរឈរ              |
| SQL         | Language for database operations | ភាសាសម្រាប់ធ្វើការជាមួយ Database  |
| Query       | Request data                     | ស្នើសុំទិន្នន័យ                   |
| Primary Key | Uniquely identifies a row        | សម្គាល់ Row មួយឱ្យមានតែមួយ        |
| Foreign Key | Connects tables                  | ភ្ជាប់ Table                      |
| Index       | Improves lookup performance      | ជួយឱ្យស្វែងរកទិន្នន័យលឿន          |
| View        | Virtual table                    | Table និម្មិត                     |
| Sequence    | Generates numbers                | បង្កើតលេខតាមលំដាប់                |
| Transaction | Group of database operations     | ក្រុមប្រតិបត្តិការ Database       |
| Procedure   | Stored program                   | Program ដែលរក្សាទុកក្នុង Database |
| Function    | Returns a value                  | បញ្ចេញតម្លៃមួយ                    |
| Trigger     | Automatically executes on events | ដំណើរការដោយស្វ័យប្រវត្តិតាម Event |
| Package     | Group of PL/SQL code             | ក្រុម PL/SQL ដែលពាក់ព័ន្ធគ្នា     |
| Cursor      | Processes query results          | ដំណើរការ Result របស់ Query        |
| Exception   | Handles errors                   | ដោះស្រាយ Error                    |

---

# 🇰🇭 Khmer Learning Order

បើអ្នកចាប់ផ្តើម Oracle ពីសូន្យ សូមរៀនតាមលំដាប់នេះ៖

```text
Oracle Basics
     ↓
CREATE TABLE
     ↓
INSERT
     ↓
SELECT
     ↓
WHERE
     ↓
ORDER BY
     ↓
UPDATE / DELETE
     ↓
Functions
     ↓
GROUP BY / HAVING
     ↓
JOIN
     ↓
Subquery
     ↓
CTE
     ↓
Constraints
     ↓
View
     ↓
Sequence / Identity
     ↓
Index
     ↓
Transaction
     ↓
Analytic Functions
     ↓
PL/SQL
     ↓
Cursor
     ↓
Exception
     ↓
Procedure
     ↓
Function
     ↓
Trigger
     ↓
Package
     ↓
Dynamic SQL
     ↓
Bulk Processing
     ↓
Performance
     ↓
Security
     ↓
Production Database
```

---

# 🚀 Final Goal

After completing this guide, you should be able to build systems such as:

```text
E-Commerce
Banking
Inventory
School Management
Hospital Management
Employee Management
POS
Accounting
ERP
CRM
```

Typical architecture:

```text
Frontend
   │
   ▼
Backend API
   │
   ▼
Service Layer
   │
   ▼
Oracle Database
   │
   ├── Tables
   ├── Views
   ├── Indexes
   ├── Procedures
   ├── Functions
   ├── Packages
   └── Triggers
```

---

# 📌 Recommended Project Structure

```text
oracle-project/
│
├── README.md
│
├── database/
│   ├── 01_tables.sql
│   ├── 02_constraints.sql
│   ├── 03_sequences.sql
│   ├── 04_indexes.sql
│   ├── 05_views.sql
│   ├── 06_data.sql
│   ├── 07_procedures.sql
│   ├── 08_functions.sql
│   ├── 09_triggers.sql
│   └── 10_packages.sql
│
└── docs/
    ├── architecture.md
    ├── database-design.md
    └── api-database.md
```

---

# ⭐ Important Oracle Concepts

```text
SQL
├── DDL
├── DML
├── DQL
├── DCL
└── TCL

Oracle Database
├── Tables
├── Views
├── Indexes
├── Sequences
├── Constraints
└── Transactions

PL/SQL
├── Variables
├── Conditions
├── Loops
├── Cursors
├── Exceptions
├── Procedures
├── Functions
├── Triggers
└── Packages

Advanced
├── CTE
├── Analytic Functions
├── Window Functions
├── Dynamic SQL
├── Bulk Processing
├── JSON
├── Query Optimization
└── Security
```

---

# 🏁 Conclusion

Oracle Database is much more than writing `SELECT` statements.

A professional Oracle developer should understand:

```text
SQL
+
Database Design
+
Constraints
+
Transactions
+
Indexes
+
PL/SQL
+
Performance
+
Security
```

Start with simple SQL, build real projects, then move into PL/SQL and performance optimization.

**Learn → Practice → Build → Optimize → Deploy**

---

## 📄 License

This documentation is free to use for learning and personal projects.

---
