### **1. Wound and Wait**

**Rule**: If a **younger** transaction wants to access data locked by an **older** one, the **younger is killed**.

#### Example:

```sql
-- Transaction T1 (Older)
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;

-- Transaction T2 (Younger)
BEGIN;
UPDATE accounts SET balance = balance + 100 WHERE id = 1;
-- Conflict: id = 1 is locked by T1
-- Action: T2 is killed and must restart later
```

**Why?** To prevent deadlock by removing the younger transaction before it blocks the older one.

---

### **2. Wait and Die**

**Rule**: If a **younger** transaction wants data held by an **older** one, it **waits**. But if an **older** transaction requests data held by a **younger**, the **older is killed**.

#### Example:

```sql
-- Transaction T1 (Older)
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;

-- Transaction T2 (Younger)
BEGIN;
UPDATE accounts SET balance = balance + 100 WHERE id = 1;
-- T2 is younger, so it waits for T1 to finish

-- If T1 later requests data locked by T2
-- Then T1 is killed (older dies if blocked by younger)
```

**Why?** Older transactions don’t wait for younger ones to avoid deadlock.

---

### **3. Partial Rollback**

**Rule**: Only the part of the transaction that fails is rolled back. Others continue if the DB supports it (e.g., using savepoints).

#### Example:

```sql
BEGIN;

SAVEPOINT step1;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;

SAVEPOINT step2;
UPDATE accounts SET balance = balance / 0 WHERE id = 2; -- Error: Division by zero

ROLLBACK TO step2; -- Only undo step2
COMMIT;
```

**Result**: `id = 1` update is saved, only the second update is rolled back.

---

### **4. Full Rollback**

**Rule**: If any part fails, the entire transaction is undone.

#### Example:

```sql
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance / 0 WHERE id = 2; -- Error

ROLLBACK; -- Entire transaction is undone
```

**Result**: Both changes are discarded, as the transaction failed.
