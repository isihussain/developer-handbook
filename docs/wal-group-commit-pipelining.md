# WAL: Group Commit & fsync Pipelining Architecture

Database engines batch concurrent transaction flush requests into a single hardware `fsync` call to maximize NVMe IOPS efficiency and eliminate write stall bottlenecks.

```mermaid
sequenceDiagram
    participant T1 as Thread 1 (Leader)
    participant T2 as Thread 2 (Follower)
    participant T3 as Thread 3 (Follower)
    participant Disk as NVMe Storage Controller
    T1->>T1: Acquire Group Lock (Leader)
    T2->>T1: Enqueue TX Commit 2
    T3->>T1: Enqueue TX Commit 3
    Note over T1: Flushes combined buffer (TX 1 + 2 + 3)
    T1->>Disk: pwritev() + fdatasync()
    Disk-->>T1: Hardware Flush Acknowledged
    T1-->>T2: Wake up follower thread (Committed)
    T1-->>T3: Wake up follower thread (Committed)
```

---

## 1. Optimization Mechanics

* **Slot Ring Queues:** Non-blocking atomic ring buffers buffer commit tokens. The first arriving worker thread becomes the leader; subsequent threads join the pending sync batch.
* **Delayed Sync Tuning:** Configurable thresholds (`commit_delay`, `commit_siblings`) pause the leader for several microseconds to batch additional concurrent transactions into one barrier.
