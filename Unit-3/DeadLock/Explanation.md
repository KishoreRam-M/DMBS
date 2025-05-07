## **What is a Deadlock?**

A **deadlock** happens when **two or more transactions are waiting for each other** to release resources, and none of them can move forward.

Imagine two people trying to cross a narrow bridge from opposite sides — neither one can pass unless the other steps back, but both refuse to do so. That’s a deadlock!

---

## **Four Things Needed for Deadlock to Happen**

### 1. **Mutual Exclusion**

Only one transaction can use a resource at a time.

**Example:**

```sql
T1: locks account 1
T2: locks account 2
```

---

### 2. **Hold and Wait**

A transaction holds one resource and waits for another.

**Example:**

```sql
T1: has account 1, waiting for account 2
T2: has account 2, waiting for account 1
```

---

### 3. **No Preemption**

You can’t force a transaction to release what it’s holding — it must finish or give up.

---

### 4. **Circular Wait**

A circle of transactions is waiting on each other.

**Example:**

* T1 → waits for T2
* T2 → waits for T1
  That’s a deadlock!

---

## **Simple SQL Example**

Let's say we have a table:

```sql
CREATE TABLE accounts (
  id INT PRIMARY KEY,
  balance INT
);

INSERT INTO accounts VALUES (1, 1000), (2, 1000);
```

### **Transaction 1 (T1):**

```sql
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;  -- locks account 1
UPDATE accounts SET balance = balance + 100 WHERE id = 2;  -- waits for account 2
```

### **Transaction 2 (T2):**

```sql
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE id = 2;  -- locks account 2
UPDATE accounts SET balance = balance + 100 WHERE id = 1;  -- waits for account 1
```

Now both T1 and T2 are **stuck**, waiting for each other. This is a **deadlock**.

---

## **How to Prevent Deadlocks (Easy Rules)**

1. **Lock in Same Order**
   Always access resources in the same order:

   ```sql
   -- Lock account 1, then account 2
   ```

2. **Request All at Once**
   Lock everything you need early:

   ```sql
   SELECT * FROM accounts WHERE id IN (1, 2) FOR UPDATE;
   ```

3. **Set a Timeout**
   Let the system cancel a transaction if it waits too long.
