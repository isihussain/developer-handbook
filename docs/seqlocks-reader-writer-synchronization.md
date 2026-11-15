# Sequential Locks (SeqLocks) Reader-Writer Synchronization

SeqLocks provide lock-free, zero-wait read access for small, copyable data structures by using an atomic sequence counter. Readers never block writers, but re-try reads if a sequence parity shift indicates concurrent modification.

```mermaid
sequenceDiagram
    participant W as Writer Thread
    participant Seq as Sequence Counter (even = clean, odd = locked)
    participant R as Reader Thread

    R->>Seq: Read start sequence S1 (must be even)
    R->>R: Copy data fields payload into local registers
    W->>Seq: write_seqlock: Increment Seq (now odd)
    W->>W: Mutate data fields in memory
    W->>Seq: write_sequnlock: Increment Seq (now even S2)
    R->>Seq: Read end sequence S2 (check if S1 == S2)
    Note over R: S1 != S2! Data was torn: Retry loop
```

---

## 1. Sequence Parity Contract

* **Even Number:** Memory state is consistent and stable for reading.
* **Odd Number:** Writer is currently modifying data; readers must loop or spin until completion.
