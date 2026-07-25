# Oracle Database — Writing Data, Beginner to Advanced

A complete, practical guide to **writing data in Oracle Database** — from your first `INSERT` to production-grade bulk operations, transactions, triggers, and error handling.

Examples use standard **Oracle SQL** and **PL/SQL** (works in SQL*Plus, SQLcl, SQL Developer). A **Node.js (`node-oracledb`)** section is included for application-level writes.

```bash
npm install oracledb
```

---

## Table of Contents

1. [Setup & Connection](#1-setup--connection)
2. [Beginner: Creating a Table](#2-beginner-creating-a-table)
3. [Beginner: Inserting Rows](#3-beginner-inserting-rows)
4. [Beginner: Basic Updates](#4-beginner-basic-updates)
5. [Beginner: Deleting Rows](#5-beginner-deleting-rows)
6. [Intermediate: MERGE (Upsert)](#6-intermediate-merge-upsert)
7. [Intermediate: Sequences & Identity Columns](#7-intermediate-sequences--identity-columns)
8. [Intermediate: Multi-Table Inserts](#8-intermediate-multi-table-inserts)
9. [Intermediate: RETURNING INTO](#9-intermediate-returning-into)
10. [Advanced: Bulk Operations (FORALL / BULK COLLECT)](#10-advanced-bulk-operations-forall--bulk-collect)
11. [Advanced: Transactions & Savepoints](#11-advanced-transactions--savepoints)
12. [Advanced: Triggers for Auditing](#12-advanced-triggers-for-auditing)
13. [Advanced: Constraints & Validation](#13-advanced-constraints--validation)
14. [Advanced: Node.js (node-oracledb) Application Writes](#14-advanced-nodejs-node-oracledb-application-writes)
15. [Advanced: Error Handling in PL/SQL](#15-advanced-error-handling-in-plsql)
16. [Best Practices Cheat Sheet](#16-best-practices-cheat-sheet)

---

## 1. Setup & Connection

### SQL*Plus / SQLcl

```sql
-- Connect to a database
sqlplus username/password@//hostname:1521/service_name
```

### Node.js Connection Pool

```javascript
// db.js
const oracledb = require("oracledb");
oracledb.outFormat = oracledb.OUT_FORMAT_OBJECT;

let pool;

async function initPool() {
  pool = await oracledb.createPool({
    user: process.env.ORACLE_USER,
    password: process.env.ORACLE_PASSWORD,
    connectString: process.env.ORACLE_CONNECT_STRING, // e.g. "localhost:1521/XEPDB1"
    poolMin: 2,
    poolMax: 10,
    poolIncrement: 1
  });
  console.log("Oracle connection pool created");
}

async function getConnection() {
  if (!pool) throw new Error("Pool not initialized. Call initPool() first.");
  return pool.getConnection();
}

module.exports = { initPool, getConnection };
```

---

## 2. Beginner: Creating a Table

```sql
CREATE TABLE users (
    user_id     NUMBER GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    name        VARCHAR2(100) NOT NULL,
    email       VARCHAR2(150) NOT NULL UNIQUE,
    age         NUMBER(3),
    status      VARCHAR2(20) DEFAULT 'active',
    created_at  TIMESTAMP DEFAULT SYSTIMESTAMP
);
```

---

## 3. Beginner: Inserting Rows

### 3.1 Insert a Single Row

```sql
INSERT INTO users (name, email, age)
VALUES ('Alice Johnson', 'alice@example.com', 28);

COMMIT;
```

### 3.2 Insert Multiple Rows (single statement, Oracle style)

```sql
INSERT ALL
    INTO users (name, email, age) VALUES ('Bob Smith', 'bob@example.com', 34)
    INTO users (name, email, age) VALUES ('Carla Diaz', 'carla@example.com', 22)
    INTO users (name, email, age) VALUES ('David Lee', 'david@example.com', 41)
SELECT * FROM dual;

COMMIT;
```

### 3.3 Insert from a Query

```sql
INSERT INTO archived_users (name, email, age)
SELECT name, email, age
FROM users
WHERE status = 'inactive';

COMMIT;
```

> ⚠️ In Oracle, writes are **not durable until `COMMIT`**. Until then, other sessions cannot see your changes, and a `ROLLBACK` discards them entirely.

---

## 4. Beginner: Basic Updates

```sql
-- Update a single row
UPDATE users
SET age = 29
WHERE email = 'alice@example.com';

-- Update many rows
UPDATE users
SET status = 'inactive'
WHERE created_at < TIMESTAMP '2024-01-01 00:00:00';

COMMIT;
```

Check how many rows were affected:

```sql
UPDATE users SET status = 'inactive' WHERE age > 60;
DBMS_OUTPUT.PUT_LINE(SQL%ROWCOUNT || ' rows updated'); -- inside PL/SQL block
```

---

## 5. Beginner: Deleting Rows

```sql
-- Delete a single matching row
DELETE FROM users WHERE email = 'bob@example.com';

-- Delete many rows
DELETE FROM users WHERE status = 'inactive';

-- Delete ALL rows fast, without generating per-row undo (cannot be rolled back after commit)
TRUNCATE TABLE staging_logs;

COMMIT;
```

| Command | Logged? | Can Rollback? | Resets Identity? | Fires Triggers? |
|---|---|---|---|---|
| `DELETE` | Yes (full undo) | Yes, until commit | No | Yes |
| `TRUNCATE` | Minimal | No (DDL, auto-commits) | Yes | No |

---

## 6. Intermediate: MERGE (Upsert)

`MERGE` inserts a new row if no match exists, or updates it if one does — Oracle's native upsert.

```sql
MERGE INTO user_preferences tgt
USING (SELECT 1042 AS user_id, 'dark' AS theme FROM dual) src
ON (tgt.user_id = src.user_id)
WHEN MATCHED THEN
    UPDATE SET tgt.theme = src.theme,
               tgt.updated_at = SYSTIMESTAMP
WHEN NOT MATCHED THEN
    INSERT (user_id, theme, created_at)
    VALUES (src.user_id, src.theme, SYSTIMESTAMP);

COMMIT;
```

### MERGE with DELETE clause

```sql
MERGE INTO product_inventory tgt
USING incoming_stock src
ON (tgt.sku = src.sku)
WHEN MATCHED THEN
    UPDATE SET tgt.quantity = src.quantity
    DELETE WHERE src.quantity = 0
WHEN NOT MATCHED THEN
    INSERT (sku, quantity) VALUES (src.sku, src.quantity);

COMMIT;
```

---

## 7. Intermediate: Sequences & Identity Columns

### Identity Column (modern, Oracle 12c+, recommended)

```sql
CREATE TABLE orders (
    order_id    NUMBER GENERATED ALWAYS AS IDENTITY START WITH 1000 INCREMENT BY 1,
    order_code  VARCHAR2(20) NOT NULL,
    total       NUMBER(10,2)
);

INSERT INTO orders (order_code, total) VALUES ('ORD-0001', 129.99);
COMMIT;
```

### Classic Sequence (still widely used, more flexible)

```sql
CREATE SEQUENCE order_seq
    START WITH 1000
    INCREMENT BY 1
    NOCACHE;

INSERT INTO orders (order_id, order_code, total)
VALUES (order_seq.NEXTVAL, 'ORD-0002', 59.50);

COMMIT;

-- Get the value just used in this session
SELECT order_seq.CURRVAL FROM dual;
```

---

## 8. Intermediate: Multi-Table Inserts

Split one source row into several target tables in a single pass — useful for ETL/staging.

```sql
INSERT ALL
    WHEN total >= 1000 THEN
        INTO orders_high_value (order_id, total)
        VALUES (order_id, total)
    WHEN total < 1000 THEN
        INTO orders_standard (order_id, total)
        VALUES (order_id, total)
SELECT order_id, total FROM staging_orders;

COMMIT;
```

---

## 9. Intermediate: RETURNING INTO

Capture generated or modified values without a second round trip — essential in PL/SQL.

```sql
DECLARE
    v_id     users.user_id%TYPE;
    v_email  users.email%TYPE;
BEGIN
    INSERT INTO users (name, email, age)
    VALUES ('Erin Walsh', 'erin@example.com', 31)
    RETURNING user_id, email INTO v_id, v_email;

    DBMS_OUTPUT.PUT_LINE('New user_id: ' || v_id || ' (' || v_email || ')');

    COMMIT;
END;
/
```

---

## 10. Advanced: Bulk Operations (FORALL / BULK COLLECT)

For loading thousands of rows efficiently, avoid row-by-row loops — use `FORALL` to send the whole batch to the SQL engine in one context switch.

```sql
DECLARE
    TYPE t_id_list    IS TABLE OF users.user_id%TYPE;
    TYPE t_email_list IS TABLE OF users.email%TYPE;

    v_ids    t_id_list    := t_id_list();
    v_emails t_email_list := t_email_list();
BEGIN
    -- Populate the collections (normally from a cursor, file, or API payload)
    v_ids.EXTEND(3);
    v_emails.EXTEND(3);

    v_ids(1) := 101; v_emails(1) := 'user101@example.com';
    v_ids(2) := 102; v_emails(2) := 'user102@example.com';
    v_ids(3) := 103; v_emails(3) := 'user103@example.com';

    FORALL i IN 1 .. v_ids.COUNT SAVE EXCEPTIONS
        UPDATE users
        SET email = v_emails(i)
        WHERE user_id = v_ids(i);

    COMMIT;

EXCEPTION
    WHEN OTHERS THEN
        FOR i IN 1 .. SQL%BULK_EXCEPTIONS.COUNT LOOP
            DBMS_OUTPUT.PUT_LINE(
                'Error at index ' || SQL%BULK_EXCEPTIONS(i).ERROR_INDEX ||
                ': ' || SQLERRM(-SQL%BULK_EXCEPTIONS(i).ERROR_CODE)
            );
        END LOOP;
        ROLLBACK;
END;
/
```

### BULK COLLECT — Reading and Rewriting in Batches

```sql
DECLARE
    TYPE t_user_tab IS TABLE OF users%ROWTYPE;
    v_users t_user_tab;

    CURSOR c_inactive IS
        SELECT * FROM users WHERE status = 'inactive';
BEGIN
    OPEN c_inactive;
    LOOP
        FETCH c_inactive BULK COLLECT INTO v_users LIMIT 500; -- batch size
        EXIT WHEN v_users.COUNT = 0;

        FORALL i IN 1 .. v_users.COUNT
            UPDATE users
            SET status = 'archived'
            WHERE user_id = v_users(i).user_id;

        COMMIT; -- commit per batch to limit undo growth
    END LOOP;
    CLOSE c_inactive;
END;
/
```

---

## 11. Advanced: Transactions & Savepoints

```sql
DECLARE
    v_balance NUMBER;
BEGIN
    -- Debit the source account
    UPDATE accounts
    SET balance = balance - 500
    WHERE account_id = 1001
    RETURNING balance INTO v_balance;

    IF v_balance < 0 THEN
        RAISE_APPLICATION_ERROR(-20001, 'Insufficient funds');
    END IF;

    SAVEPOINT after_debit;

    -- Credit the destination account
    UPDATE accounts
    SET balance = balance + 500
    WHERE account_id = 1002;

    -- Record the transfer
    INSERT INTO transfers (from_account, to_account, amount, created_at)
    VALUES (1001, 1002, 500, SYSTIMESTAMP);

    COMMIT;

EXCEPTION
    WHEN OTHERS THEN
        ROLLBACK TO after_debit; -- or ROLLBACK for the whole transaction
        RAISE;
END;
/
```

**Key points:**
- A transaction starts implicitly with the first DML statement and ends at `COMMIT` or `ROLLBACK`.
- `SAVEPOINT` lets you roll back part of a transaction without discarding everything.
- Oracle's default isolation level is `READ COMMITTED`; use `SET TRANSACTION ISOLATION LEVEL SERIALIZABLE` for stricter guarantees when needed.

---

## 12. Advanced: Triggers for Auditing

Automatically capture write activity without changing application code.

```sql
CREATE TABLE users_audit (
    audit_id    NUMBER GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    user_id     NUMBER,
    action      VARCHAR2(10),
    old_email   VARCHAR2(150),
    new_email   VARCHAR2(150),
    changed_at  TIMESTAMP DEFAULT SYSTIMESTAMP,
    changed_by  VARCHAR2(50)
);

CREATE OR REPLACE TRIGGER trg_users_audit
AFTER INSERT OR UPDATE OR DELETE ON users
FOR EACH ROW
DECLARE
    v_action VARCHAR2(10);
BEGIN
    IF INSERTING THEN
        v_action := 'INSERT';
    ELSIF UPDATING THEN
        v_action := 'UPDATE';
    ELSE
        v_action := 'DELETE';
    END IF;

    INSERT INTO users_audit (user_id, action, old_email, new_email, changed_by)
    VALUES (
        NVL(:NEW.user_id, :OLD.user_id),
        v_action,
        :OLD.email,
        :NEW.email,
        SYS_CONTEXT('USERENV', 'SESSION_USER')
    );
END;
/
```

---

## 13. Advanced: Constraints & Validation

```sql
CREATE TABLE orders (
    order_id     NUMBER GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    customer_id  NUMBER NOT NULL,
    total        NUMBER(10,2) NOT NULL,
    status       VARCHAR2(20) DEFAULT 'pending',

    CONSTRAINT chk_total_positive CHECK (total >= 0),
    CONSTRAINT chk_status_valid
        CHECK (status IN ('pending', 'shipped', 'delivered', 'cancelled')),
    CONSTRAINT fk_orders_customer
        FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);
```

A write that violates a constraint raises an Oracle error immediately, before anything is committed:

| Error Code | Meaning |
|---|---|
| `ORA-00001` | Unique constraint violated (duplicate value) |
| `ORA-01400` | Cannot insert NULL into a `NOT NULL` column |
| `ORA-02290` | Check constraint violated |
| `ORA-02291` | Foreign key constraint violated — parent key not found |
| `ORA-02292` | Foreign key violated — child record exists (on delete) |

---

## 14. Advanced: Node.js (node-oracledb) Application Writes

### Single Insert with Bind Variables (always use binds — never string-concatenate SQL)

```javascript
async function insertUser(connection, name, email, age) {
  const result = await connection.execute(
    `INSERT INTO users (name, email, age)
     VALUES (:name, :email, :age)
     RETURNING user_id INTO :id`,
    {
      name,
      email,
      age,
      id: { dir: require("oracledb").BIND_OUT, type: require("oracledb").NUMBER }
    },
    { autoCommit: true }
  );

  console.log(`Inserted user_id: ${result.outBinds.id[0]}`);
}
```

### Batch Insert with `executeMany` (equivalent to FORALL from the app layer)

```javascript
async function insertUsersBatch(connection, users) {
  const oracledb = require("oracledb");

  const result = await connection.executeMany(
    `INSERT INTO users (name, email, age) VALUES (:name, :email, :age)`,
    users, // [{ name, email, age }, { name, email, age }, ...]
    {
      autoCommit: true,
      batchErrors: true, // continue past row-level errors, collect them
      bindDefs: {
        name:  { type: oracledb.STRING, maxSize: 100 },
        email: { type: oracledb.STRING, maxSize: 150 },
        age:   { type: oracledb.NUMBER }
      }
    }
  );

  console.log(`Rows affected: ${result.rowsAffected}`);
  if (result.batchErrors) {
    result.batchErrors.forEach(err =>
      console.error(`Row ${err.offset} failed: ${err.message}`)
    );
  }
}
```

### Transaction Across Multiple Statements

```javascript
async function transferFunds(pool, fromId, toId, amount) {
  const connection = await pool.getConnection();
  try {
    await connection.execute(
      `UPDATE accounts SET balance = balance - :amt WHERE account_id = :id AND balance >= :amt`,
      { amt: amount, id: fromId },
      { autoCommit: false }
    );

    await connection.execute(
      `UPDATE accounts SET balance = balance + :amt WHERE account_id = :id`,
      { amt: amount, id: toId },
      { autoCommit: false }
    );

    await connection.execute(
      `INSERT INTO transfers (from_account, to_account, amount) VALUES (:f, :t, :a)`,
      { f: fromId, t: toId, a: amount },
      { autoCommit: false }
    );

    await connection.commit();
    console.log("Transfer committed");
  } catch (error) {
    await connection.rollback();
    console.error("Transfer rolled back:", error.message);
    throw error;
  } finally {
    await connection.close();
  }
}
```

---

## 15. Advanced: Error Handling in PL/SQL

```sql
DECLARE
    e_insufficient_funds EXCEPTION;
    PRAGMA EXCEPTION_INIT(e_insufficient_funds, -20001);
BEGIN
    UPDATE accounts
    SET balance = balance - 1000
    WHERE account_id = 55 AND balance >= 1000;

    IF SQL%ROWCOUNT = 0 THEN
        RAISE_APPLICATION_ERROR(-20001, 'Insufficient funds or account not found');
    END IF;

    COMMIT;

EXCEPTION
    WHEN e_insufficient_funds THEN
        ROLLBACK;
        DBMS_OUTPUT.PUT_LINE('Business error: ' || SQLERRM);

    WHEN DUP_VAL_ON_INDEX THEN
        ROLLBACK;
        DBMS_OUTPUT.PUT_LINE('Duplicate value error: ' || SQLERRM);

    WHEN OTHERS THEN
        ROLLBACK;
        DBMS_OUTPUT.PUT_LINE('Unexpected error [' || SQLCODE || ']: ' || SQLERRM);
        RAISE; -- re-raise so the caller/app layer knows it failed
END;
/
```

**Common named exceptions worth handling explicitly:**

| Exception | Raised When |
|---|---|
| `DUP_VAL_ON_INDEX` | Unique constraint / index violation |
| `NO_DATA_FOUND` | A `SELECT INTO` returns zero rows |
| `TOO_MANY_ROWS` | A `SELECT INTO` returns more than one row |
| `VALUE_ERROR` | Data type conversion/truncation error |
| `OTHERS` | Catch-all — always log `SQLCODE`/`SQLERRM` before handling |

---

## 16. Best Practices Cheat Sheet

- ✅ Always use **bind variables** (`:name`) — never concatenate user input into SQL strings (prevents SQL injection and enables statement caching).
- ✅ Use `MERGE` for upserts instead of "check-then-insert-or-update" logic (avoids race conditions).
- ✅ Prefer `IDENTITY` columns for new schemas; use sequences when you need more control (e.g. shared across tables).
- ✅ Use `FORALL`/`executeMany` for bulk writes — row-by-row loops are dramatically slower.
- ✅ Commit in batches during large bulk loads (e.g. every 500–1000 rows) to control undo/rollback segment growth.
- ✅ Use `RETURNING ... INTO` to avoid a second round trip after INSERT/UPDATE.
- ✅ Wrap multi-step writes in a transaction with `SAVEPOINT`s for partial rollback control.
- ✅ Add `CHECK`, `NOT NULL`, and `FOREIGN KEY` constraints — catch bad data at the database, not just the app layer.
- ✅ Handle `OTHERS` last in exception blocks, and always log `SQLCODE`/`SQLERRM` before rolling back.
- ❌ Don't forget `COMMIT` — uncommitted changes are invisible to other sessions and vanish on disconnect.
- ❌ Don't use `TRUNCATE` on a table you might need to roll back — it's DDL and auto-commits immediately.
- ❌ Don't rely on `SELECT MAX(id)+1` for generating keys — use `IDENTITY`/sequences to avoid race conditions.

---

## License

Free to use in any project — copy, adapt, and drop straight into your own `README.md`.
