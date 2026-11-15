# SQLite: Write-Ahead Logging vs Rollback Journal

SQLite supports two crash-recovery modes: Rollback Journal (modifies database pages directly after creating undo logs) and WAL (appends commits to a separate write log).

```mermaid
flowchart LR
    subgraph RollbackMode[Rollback Journal Mode]
        T_R[Transaction] -->|Write original pages| JournalFile[Journal File: Undo Log]
        JournalFile -->|Overwrites in place| DB_R[Database File]
    end
    subgraph WALMode[WAL Mode: Concurrent Reads]
        T_W[Transaction] -->|Appends new frames| WALFile[WAL File: .db-wal]
        WALFile -.->|Async Checkpoint| DB_W[Database File]
    end
```

---

## 1. Tradeoffs

| Feature | Rollback Journal Mode | WAL Mode (`PRAGMA journal_mode=WAL;`) |
|---|---|---|
| **Concurrency** | Single writer OR multiple readers | Single writer AND multiple readers simultaneously |
| **Write IOPS** | 2 fsync calls per write transaction | 1 fsync call per commit (batchable) |
| **Files on Disk** | `.db` and temporary `-journal` | `.db`, `.db-wal`, and shared-memory `.db-shm` |
| **Network Filesystems**| Compatible | Incompatible: requires shared-memory primitives |
