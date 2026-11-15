# File Systems: ext4 Journaling Block Device (JBD2) Architecture

The Journaling Block Device 2 (JBD2) protects ext4 file systems against power loss and system crashes by writing metadata modifications to a circular journal partition before committing blocks to disk.

```mermaid
sequenceDiagram
    participant VFS as Linux VFS write()
    participant JBD2 as JBD2 Journal Transaction
    participant Disk as Physical Storage Extents
    VFS->>JBD2: Start Transaction (Appends metadata descriptors)
    Note over JBD2: Flushes transaction blocks to Journal on disk
    JBD2->>Disk: Issues Commit Record with Barrier / Flush
    Note over JBD2: Transaction Committed safely in Journal
    JBD2->>Disk: Checkpoint Phase: Asynchronously writes blocks to final inode table
```

---

## 1. Journaling Modes

| Mode | Mount Option | Data Invariant | Performance |
|---|---|---|---|
| **Data Journaling** | `data=journal` | Both user data and metadata pass through the journal | Lowest write throughput; maximum crash consistency |
| **Ordered** (Default) | `data=ordered` | User data is flushed to disk before metadata commits | High throughput; prevents stale file data leaks |
| **Writeback** | `data=writeback` | Metadata journals; user data writes proceed asynchronously | Highest throughput; potential stale data exposed after crash |
