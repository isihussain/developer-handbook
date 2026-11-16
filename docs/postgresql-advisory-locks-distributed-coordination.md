# Concurrency Control: PostgreSQL Application Advisory Locks

PostgreSQL Advisory Locks allow applications to acquire arbitrary cluster-wide mutexes without locking physical table rows or rows that do not exist yet.

```mermaid
sequenceDiagram
    participant Worker1
    participant Postgres as PostgreSQL Engine (Advisory Lock Table in RAM)
    participant Worker2

    Worker1->>Postgres: pg_try_advisory_lock(10042)
    Postgres-->>Worker1: TRUE (Lock Acquired!)
    Worker2->>Postgres: pg_try_advisory_lock(10042)
    Postgres-->>Worker2: FALSE (Lock Contended!)
    Note over Worker1: Executes batch invoice reconciliation
    Worker1->>Postgres: pg_advisory_unlock(10042)
```

---

## 1. Session vs Transaction Scope
- `pg_advisory_lock()`: Survives transaction boundaries; released explicitly or on connection disconnect.
- `pg_advisory_xact_lock()`: Automatically released upon transaction `COMMIT` or `ROLLBACK`.
