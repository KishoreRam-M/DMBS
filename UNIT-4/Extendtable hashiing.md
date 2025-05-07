# 🌱 Extendable Hashing – Simplified 4-Mark Answer

## 📌 What

**Extendable Hashing** is a dynamic hashing technique where the **directory and buckets grow as needed**, avoiding rehashing the entire dataset when a bucket overflows.

---

## ❓ Why Use It

* Handles unpredictable data growth smoothly
* Keeps search time fast (**near O(1)**)
* Avoids expensive full-table rehashing

---

## ⏰ When to Use

* When data grows continuously (e.g., databases, file systems)
* When fast, direct access is needed for large datasets

---

## 📍 Where It's Used

* Database indexing systems
* File systems (e.g., UNIX)
* Disk-based applications

---

## ⚙️ How It Works (Step-by-Step)

1. **Directory** holds pointers to **buckets**
2. Each bucket has a **local depth** (bits it uses)
3. Directory has a **global depth** (max bits used)
4. On **overflow**:

   * Bucket is **split**
   * If needed, the **directory doubles**
   * Only the **overflowing bucket** is split

---

## 🔢 Example (Keys: 8, 24, 56, 40, 72, 16)

**Assume: bucket size = 2**

### ✅ Initial State (Global Depth = 1)

```
Directory:
0 → Bucket A: [ ]
1 → Bucket B: [ ]
```

**Insert 8** → binary `1000` → last 1 bit = `0` → Bucket A: `[8]`
**Insert 24** → `11000` → last 1 bit = `0` → Bucket A: `[8, 24]` (Full, OK)
**Insert 56** → `111000` → last 1 bit = `0` → Bucket A OVERFLOWS

---

### 🔁 Split Bucket A

* Local Depth (A) ↑ to 2
* Global Depth ↑ to 2 → Directory doubles

```
Directory:
00 → Bucket A1: [8, 56]
01 → Empty
10 → Bucket A2: [24]
11 → Empty
```

**Insert 40** → `101000` → last 2 bits = `00` → Bucket A1 OVERFLOWS

---

### 🔁 Split Again

* Local Depth (A1) ↑ to 3
* Global Depth ↑ to 3 → Directory doubles again

```
Directory:
000 → Bucket A1a: [8, 40]
010 → Bucket A2:  [24]
100 → Bucket A1b: [56]
Others → Empty
```

---

## 📊 Diagram (Text-based)

```
Global Depth = 3

Directory:
000 → [8, 40]
010 → [24]
100 → [56]
Others → empty or shared
```

---

## 🧠 Key Concepts

| Term             | Meaning                                                          |
| ---------------- | ---------------------------------------------------------------- |
| **Global Depth** | Bits used by the **directory** to choose a bucket                |
| **Local Depth**  | Bits used by a **specific bucket** to manage its contents        |
| **Bucket**       | Stores keys with matching binary suffixes (based on local depth) |
| **Split**        | Happens when a bucket overflows; may increase directory size     |

---

## 🧠 Simple Analogy

* **Directory = Cupboard**
* **Buckets = Drawers**
* **Keys = Books with labels**
* **Global Depth = Number of label digits cupboard checks**
* **Local Depth = Number of digits drawer uses to sort**

---

## ✅ Final Takeaways

* Splits **only the overflowing bucket**
* **Directory doubles only when needed**
* Lookup remains **fast**
* Avoids complete rehashing
* **Perfect for large, growing datasets**

---

Would you like me to generate a **GIF animation**, **printable PDF**, or **interactive diagram** for this too?
