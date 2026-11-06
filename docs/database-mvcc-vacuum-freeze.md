# PostgreSQL MVCC: Vacuum Freeze & Transaction Wraparound

PostgreSQL allocates 32-bit transaction identifiers (`XID`), cycling every $2^{31}$ transactions.

---

## 1. Wraparound Failure Mode

Without mitigation, older committed rows appear in the "future" and become invisible. `VACUUM FREEZE` sets the `frozen` bit on rows, marking them permanently older than all active transactions.
