### **What?**

In a database system, **data is stored in files**, and **each file consists of records**.
A **record** is a group of related fields (columns), and a **file** is a collection of records (rows).

---

### **Why? (Justification)**

* A **database** internally organizes data using files for **efficient storage and retrieval**.
* Each **table** is stored as a file.
* These files are organized using various **file organization techniques**, which decide how records are placed and accessed.

---

### **Classification of File Organization Methods:**

| Type                    | Description                                            | Example Use Case                          |
| ----------------------- | ------------------------------------------------------ | ----------------------------------------- |
| **1. Heap (Unordered)** | Records are stored in any order (as they arrive).      | Temporary tables, logs                    |
| **2. Sequential**       | Records are stored in sorted order based on a key.     | Sorted employee list                      |
| **3. Hashing**          | Records are stored based on a hash of the key.         | Fast exact match search (e.g., ID search) |
| **4. Clustered**        | Related records from different tables stored together. | Customer + Orders                         |
| **5. Indexed**          | Uses an index file for fast searching.                 | Search engines, large databases           |

---

### **When & Where Used?**

* Used in **DBMS file systems**.
* Implemented when designing **storage engines** like InnoDB (MySQL) or PostgreSQL storage.
* Found in **data warehousing, OLAP, OLTP systems**.

---

### **How? (Example)**

Let’s say you have a table `Students(id, name, dept)`.

**Heap file organization**:
Records are stored in no particular order:

```
[3, Ram, CSE], [1, Anu, ECE], [2, Kiran, MECH]
```

**Sequential file organization** (ordered by `id`):

```
[1, Anu, ECE], [2, Kiran, MECH], [3, Ram, CSE]
```

**Hashing** (hashed by `id % 3`):

```
Bucket 0: [3, Ram, CSE]  
Bucket 1: [1, Anu, ECE]  
Bucket 2: [2, Kiran, MECH]
```

---

### **Diagram**

```
Database  
  └── Table (Students)  
       └── File (students.db)  
            ├── Record 1: [1, Anu, ECE]  
            ├── Record 2: [2, Kiran, MECH]  
            └── Record 3: [3, Ram, CSE]
```
