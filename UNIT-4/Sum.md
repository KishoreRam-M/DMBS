# 🔁 Nested-Loop Join Cost Analysis –

This project evaluates the **block access cost** of performing a **nested-loop join** between two tables under strict memory constraints.

---

## 📘 Problem Statement

> A database has two tables:
- **T1**: 2000 records occupying **80 blocks**
- **T2**: 400 records occupying **20 blocks**

- The system memory can hold **only 1 block from each table at a time**
- **No index is available**
- Join condition: Every pair of records is compared (Nested-Loop Join)

---

## 🎯 Objective

Determine the **minimum number of block accesses** required to evaluate the join using **nested-loop join**, with the **best outer table selection**.

---

## 🔍 Concept: Block Nested Loop Join (BNLJ)

📌 When memory holds only **1 block from each table**:
```

Total Block Accesses = (Blocks in Outer Table × Blocks in Inner Table) + Blocks in Outer Table

```

---

## 🧠 Step-by-Step Analysis

### 🅰️ Case 1: T1 as Outer Table

- Outer Blocks = 80
- Inner Blocks = 20

```

Block Accesses = 80 × 20 + 80 = 1680

```

---

### 🅱️ Case 2: T2 as Outer Table ✅ (Best)

- Outer Blocks = 20
- Inner Blocks = 80

```

Block Accesses = 20 × 80 + 20 = 1620

```

---

## ✅ Final Answer

- ✅ Best Outer Table: **T2**
- 📊 **Minimum Block Accesses**: **`1620`**

---

## 📊 Diagram: Nested-Loop Join (1 block memory constraint)

```

for each block B1 in T2:         ← Outer loop (T2: 20 blocks)
for each block B2 in T1:     ← Inner loop (T1: 80 blocks)
compare records in B1 and B2

