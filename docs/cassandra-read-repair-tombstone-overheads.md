# Distributed Storage: Cassandra Read Repair and Tombstone Overheads

Cassandra does not delete data immediately from SSTables; instead, it writes a deletion marker called a **Tombstone** with a timestamp and GC Grace Period.

```mermaid
flowchart TD
    DeleteStmt[DELETE row WHERE id = 42] --> WriteTombstone[Write Tombstone to Memtable]
    WriteTombstone --> Flush[Flush to SSTable with gc_grace_seconds]
    subgraph ReadRepair
        ReadReq[Client Read Request] --> ScanSSTables[Scan SSTables: Must read tombstones to know row is dead!]
        ScanSSTables -->|Tombstone count > 100000| Warn[TombstoneOverwhelmingException!]
    end
```

---

## 1. GC Grace Seconds
- Tombstones must survive longer than the maximum anticipated node downtime (default 10 days) so that all replicas receive notice of deletion during repair.
