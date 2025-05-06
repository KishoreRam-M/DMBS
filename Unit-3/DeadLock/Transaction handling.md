### Simple Explanation of Transaction Handling

1. **Wound and Wait**:
   - If a younger transaction wants to use data that's already locked by an older transaction, the younger transaction is **killed** and must restart.
   - This prevents deadlocks because the younger transaction is removed before it can block the older one.

2. **Wait and Die**:
   - If a younger transaction wants to use data locked by an older transaction, the younger transaction **waits** until the older one is done.
   - But if the older transaction tries to use data locked by the younger transaction, the older one is **killed** and must restart.
   - This prevents deadlocks because the older transaction won't get stuck waiting.

3. **Partial Rollback**:
   - If something goes wrong in a transaction, only the part of the transaction that caused the problem is undone.
   - The rest of the transaction continues, which means not all progress is lost, but it requires careful tracking of what’s been done.

4. **Full Rollback**:
   - If there’s a problem or deadlock, the **entire transaction** is undone, and everything is lost.
   - This ensures that the system stays consistent, but you lose all progress made by the transaction.

---

### In Short:

- **Wound and Wait**: Younger transaction is killed to prevent deadlock.
- **Wait and Die**: Younger waits, but older gets killed if it blocks the younger.
- **Partial Rollback**: Only part of the transaction is undone.
- **Full Rollback**: The whole transaction is undone if something goes wrong.
