# 🚀 UNIT III – Database Transactions

Welcome to the ultimate guide to **Transaction Management in Databases**! This unit dives deep into how modern databases ensure consistency, handle concurrency, and recover from failures using robust transactional techniques.

---

## 🧩 What's Inside?

### 🔄 Transaction Concepts

Understand what makes transactions so powerful:

* States of a transaction: `Active → Partially Committed → Committed / Aborted`
* Real-world analogy: Like online banking—either transfer succeeds fully, or not at all!

### 💎 ACID Properties

Every reliable transaction follows **ACID**:

* 🧨 **Atomicity** – All or nothing
* 🧪 **Consistency** – Valid to valid state
* 🔒 **Isolation** – No interference
* 💾 **Durability** – Survives crashes

> 💡 Imagine booking a flight and hotel together — ACID ensures you get *both* or *none*.

---

## 📆 Schedules & Serializability

Explore how transactions are interleaved and yet remain correct:

* **Schedules**: Order of operations from different transactions
* **Conflict Serializability**: Safe parallel execution
* **View Serializability**: Logical equivalence

🧠 Think: How can multiple chefs work in one kitchen without ruining the recipe?

---

## 🧾 Transaction Support in SQL

SQL supports transactions natively:

```sql
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;
```

* Rollbacks
* Savepoints
* Isolation levels (`READ COMMITTED`, `SERIALIZABLE`, etc.)

---

## 🕹️ Why Concurrency Matters?

> Without concurrency: databases become single-lane roads
> With concurrency: databases turn into high-speed highways 🚗💨

---

## 🧱 Concurrency Control Mechanisms

### 🔐 Two Phase Locking (2PL)

* Lock first 🔒, then act
* **Strict 2PL** = No unlocks until commit

### ⏳ Timestamp Ordering

* Old is gold: older transactions get priority

### 🌈 Multiversion Concurrency Control (MVCC)

* Reads see "past versions"
* No locks, just versions

### 🔍 Validation & Snapshot Isolation

* Validate before commit
* Take a snapshot = isolated view

### 🧩 Multiple Granularity Locking

* Lock database → table → row (like zooming in 🔍)

---

## 💥 Deadlock Handling

🛑 Deadlocks = Two users waiting on each other forever

Solutions:

* 🧠 Wait-For Graphs (Detect)
* ⏱️ Timestamp Protocols (Prevent)
* 🔁 Rollback (Recover)

---

## 🔁 Recovery Techniques

### 📝 Log-Based Recovery

* Every change is logged before applied
* Supports crash recovery

### 📦 Shadow Paging

* Keep original page untouched
* Switch after commit

### 🔄 Checkpointing

* Periodic database snapshot to reduce recovery time

---

## 📚 Learn More

* **Textbooks**: Korth, Elmasri, Navathe
* **Docs**: PostgreSQL, MySQL
* **Research**: IEEE papers on concurrency & recovery

---

## 🧠 Perfect For

* 🎓 DBMS learners
* 💼 Interview prep
* 👨‍💻 SQL developers
* 🧪 System design enthusiasts

---

## 🎯 Goal

To make you say:

> "Now I **finally** understand how databases keep it all together under pressure!" 
