# 💾 ACID Properties of a Transaction

In a **Database Management System (DBMS)**, a transaction is a logical unit of work that must maintain the **ACID** properties to ensure reliability and correctness.
**ACID** stands for:

* **Atomicity**
* **Consistency**
* **Isolation**
* **Durability**

Let’s explore each of them in detail with a real-world scenario: **Transferring ₹1000 from Account A to Account B**.

---

## 🧪 1. Atomicity

> **Definition**: A transaction must be treated as a single, indivisible unit. **Either all of its operations succeed**, or **none of them do**.

### ✅ Example:

**Initial State**:

* Account A: ₹5000
* Account B: ₹3000

```sql
BEGIN TRANSACTION;

-- Step 1: Deduct ₹1000 from Account A
UPDATE accounts SET balance = balance - 1000 WHERE account_id = 'A';

-- Step 2: Add ₹1000 to Account B
UPDATE accounts SET balance = balance + 1000 WHERE account_id = 'B';

COMMIT;
```

### ❌ What if Step 2 fails?

Suppose the system crashes **after deducting from A**, but **before crediting B**.
Without Atomicity, A loses ₹1000, but B gets nothing — leading to data corruption.

**Atomicity ensures rollback**:

```sql
ROLLBACK;
```

> Result: No money is deducted from A. The transaction has no effect unless both steps succeed.

---

## 🧮 2. Consistency

> **Definition**: A transaction must transform the database from one **valid state** to another, preserving all constraints (e.g., foreign keys, totals, triggers).

### ✅ Example:

* Before Transaction:
  Total Balance = ₹5000 (A) + ₹3000 (B) = ₹8000
* After Transaction:
  Total Balance = ₹4000 (A) + ₹4000 (B) = ₹8000 ✅

There is **no data loss or duplication**. Constraints (like non-negative balances) are preserved.

> 🔒 If any rule is violated (e.g., negative balance), the DBMS will reject the transaction and roll back.

---

## 🧍 3. Isolation

> **Definition**: Each transaction must execute as if it were the **only one in the system** — even if multiple transactions run concurrently.

### ✅ Example:

Two users:

* **User 1** transfers ₹1000 from A to B.
* **User 2** checks balance of A.

If Isolation is not enforced, User 2 might see:

* A’s balance after deduction (₹4000), **before** it's committed.
* Or worse, an **in-between** (dirty read) or changing value (non-repeatable read).

**With Isolation**, User 2:

* Sees A’s balance as ₹5000 until User 1’s transaction is fully committed.

> DBMS uses **locks, timestamps, or MVCC** to isolate concurrent transactions.

---

## 🔐 4. Durability

> **Definition**: Once a transaction is **committed**, its effects are **permanent** — even in the face of crashes, power loss, or hardware failure.

### ✅ Example:

Once the ₹1000 transfer is committed:

```sql
COMMIT;
```

Even if:

* The server crashes
* The disk reboots
* Power goes out

The changes are **saved to non-volatile storage** (e.g., transaction logs or WALs — Write-Ahead Logs).

> Durability ensures that users do not lose their data after confirmation.

---

## ✅ Summary Table

| Property    | Description                                          | Real-world Effect                                                          |
| ----------- | ---------------------------------------------------- | -------------------------------------------------------------------------- |
| Atomicity   | All-or-nothing: either full success or full rollback | No partial transfer — money is not lost or duplicated                      |
| Consistency | Valid state before → Valid state after               | Total balance remains same, rules like non-negative balances are respected |
| Isolation   | No interference between concurrent transactions      | One user's transfer doesn’t affect another user’s operations               |
| Durability  | Committed data stays even after crash                | Confirmed transfers will not disappear after a server failure              |

---

## 🏦 Real-Life Analogy: ATM Transaction

> Imagine withdrawing cash from an ATM:

* Money is debited from your account (Step 1)
* Then you get the cash (Step 2)

If power goes out between steps:

* **Atomicity**: The debit is canceled (you don’t lose money)
* **Consistency**: Bank ledgers remain correct
* **Isolation**: Other users using the ATM see unaffected balances
* **Durability**: If the transaction succeeded, you can still see it on your account statement

---
