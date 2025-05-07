### **1. Simple Lock Example (Implicit Locking)**

In most databases, when you update a row, it automatically locks that row.

```sql
-- Transaction 1
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
-- Now account with id = 1 is locked
-- This lock is held until COMMIT or ROLLBACK
```

At the same time, if another transaction tries:

```sql
-- Transaction 2
BEGIN;
UPDATE accounts SET balance = balance + 100 WHERE id = 1;
-- Transaction 2 will WAIT or FAIL because Transaction 1 has the lock
```

---

### **2. Proclaiming Lock (Explicit Lock Using SELECT ... FOR UPDATE)**

This tells the database: "Lock this row now, even if I'm just reading it."

```sql
-- Transaction 1
BEGIN;
SELECT * FROM accounts WHERE id = 1 FOR UPDATE;
-- Now the row is locked, even though no update happened yet

-- Later in the same transaction
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
COMMIT;
```

If another transaction tries to lock or update the same row, it will have to **wait**.

```sql
-- Transaction 2 (at the same time)
BEGIN;
SELECT * FROM accounts WHERE id = 1 FOR UPDATE;
-- This will wait until Transaction 1 commits or rolls back
```

---

### **When to Use SELECT FOR UPDATE?**

* You want to **read a row and update it later**, and you **don’t want anyone else** to change it in between.
* It’s useful in **banking apps, ticket booking, etc.** to avoid race conditions.
