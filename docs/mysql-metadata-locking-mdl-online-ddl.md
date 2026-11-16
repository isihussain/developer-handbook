# MySQL Internals: Metadata Locks (MDL) and Table Lock Queuing

MySQL acquires Metadata Locks (MDL) during DDL statements (`ALTER TABLE`). A slow `SELECT` query can queue an `ALTER`, which subsequently blocks all future incoming `SELECT` queries!

```mermaid
sequenceDiagram
    participant Tx1 as Slow SELECT (Running for 30s: Holds Shared MDL)
    participant DDL as ALTER TABLE (Queued: Requests Exclusive MDL_EXCLUSIVE)
    participant Tx2 as Incoming Fast SELECT (Requests Shared MDL)

    Note over Tx1: SELECT * FROM orders ... (active)
    Note over DDL: ALTER TABLE orders ADD COLUMN ... (BLOCKED by Tx1!)
    Note over Tx2: SELECT * FROM orders WHERE id = 1 (BLOCKED by queued DDL!)
    Note over Tx2: Connection pool exhausts! Site-wide outage!
```

---

## 1. Safe Migration Rule
Always set a tight DDL lock wait timeout before running migrations:
```sql
SET lock_wait_timeout = 5;
ALTER TABLE orders ADD COLUMN status VARCHAR(32);
```
