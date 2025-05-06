# 🔄 Conflict Operations in Transactions

Conflict operations help determine whether a schedule is serializable and safe for concurrent execution.

---

## 📌 What Are Conflict Operations?

Two operations are said to conflict if:

- They belong to different transactions.
- They access the same data item.
- At least one of them is a write operation.

---

## ⚔️ Types of Conflict Operations

| Conflict Type        | Operation Pair         | Condition                            | Example                                   |
|----------------------|------------------------|---------------------------------------|-------------------------------------------|
| Read-Write (RW)      | Read(A) & Write(A)     | One reads, the other writes same A    | T1: Read(A), T2: Write(A)                 |
| Write-Read (WR)      | Write(A) & Read(A)     | One writes, the other reads same A    | T1: Write(A), T2: Read(A)                 |
| Write-Write (WW)     | Write(A) & Write(A)    | Both write same A                     | T1: Write(A), T2: Write(A)                |

ℹ️ These operations are not interchangeable in order. Their sequence matters for correctness.

---

## ✅ Conflict Serializable Schedule

A schedule is conflict serializable if it can be transformed into a serial schedule (no overlapping transactions) by swapping only non-conflicting operations.

🧠 Example:

```text
Schedule:
T1: Write(A)
T2: Read(A)
T2: Write(A)
