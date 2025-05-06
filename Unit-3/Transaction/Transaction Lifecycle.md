## 🧾 Transaction States & Transitions in DBMS

A **transaction** in a DBMS passes through **multiple well-defined states** to ensure **ACID properties** are maintained. Below is a complete explanation of the **states, transitions**, and **important rules**.

---

### 🔄 **Transaction States**

| State                   | Description                                                     |
| ----------------------- | --------------------------------------------------------------- |
| **Active**              | Transaction has started and is executing read/write operations. |
| **Partially Committed** | All operations are executed; system is ready to commit.         |
| **Committed**           | All changes are made permanent.                                 |
| **Failed**              | Error detected; transaction cannot proceed.                     |
| **Aborted**             | System rolls back all changes made by the transaction.          |
| **Terminated**          | Transaction is fully cleaned up and removed from the system.    |

---

### 🔁 **State Transitions**

| From → To                          | When & Why                                                        |
| ---------------------------------- | ----------------------------------------------------------------- |
| `Active → Partially Committed`     | All statements executed successfully.                             |
| `Partially Committed → Committed`  | Final actions (like writing to disk) succeed.                     |
| `Active → Failed`                  | Error occurs during execution (e.g., crash, violation, deadlock). |
| `Partially Committed → Failed`     | System crash or failure before final commit.                      |
| `Failed → Aborted`                 | System begins rollback of changes to maintain atomicity.          |
| `Aborted → Active` *(Optional)*    | Transaction is retried due to transient failure (e.g., deadlock). |
| `Committed / Aborted → Terminated` | Cleanup done; transaction is removed from tracking.               |

---

### ❗ Why **Failed → Committed** is NOT Possible

#### ✅ What Failed Means:

* A critical issue (system crash, violation, etc.) occurred.
* The transaction **is no longer reliable**.
* The database may contain **partial, uncommitted changes**.

#### 🚫 Why It Can't Be Committed:

| Reason                    | Explanation                                                              |
| ------------------------- | ------------------------------------------------------------------------ |
| **Breaks Atomicity**      | You cannot commit a transaction that didn’t complete all its operations. |
| **Breaks Consistency**    | The database might end up in an invalid or inconsistent state.           |
| **Risk of Data Loss**     | Example: If debit happens but credit doesn’t, money disappears.          |
| **Partial Changes Exist** | The transaction needs to be rolled back before terminating.              |

---

### ✅ Correct Recovery Flow After Failure:

```text
Failed → Aborted → Terminated
```

* **Failed** → Error detected
* **Aborted** → Rollback changes
* **Terminated** → Cleanup complete

> ✅ *(Optional Retry)*: A new instance of the transaction may start again, from scratch:

```text
Aborted → Active
```

---

### 💡 Real-world Analogy

> You're transferring ₹1000 from Account A to B. You debited A, but the system crashes **before** crediting B.
>
> ❌ If you *commit now*, money is lost — consistency is broken.
> ✅ Instead, you must **rollback the debit** from A to maintain atomicity.

---

### ✅ Summary Table

| State Transition                     | Allowed?     | Reason                                   |
| ------------------------------------ | ------------ | ---------------------------------------- |
| **Active → Partially Committed**     | ✅            | Transaction completed execution          |
| **Partially Committed → Committed**  | ✅            | Final commit succeeded                   |
| **Active → Failed**                  | ✅            | Error detected during execution          |
| **Partially Committed → Failed**     | ✅            | Commit failed or crash occurred          |
| **Failed → Aborted**                 | ✅            | Rollback initiated to undo changes       |
| **Failed → Committed**               | ❌            | Violates atomicity and consistency       |
| **Aborted → Active**                 | ✅ (Optional) | For retryable failures like deadlocks    |
| **Committed / Aborted → Terminated** | ✅            | Transaction ends and cleans up resources |

---
