# Database Engines: Write-Ahead Logging (WAL) Group Commit Pipelining

WAL Group Commit consolidates concurrent disk synchronization requests (`fdatasync`) across hundreds of parallel transactions into batched sequential storage writes.

```mermaid
flowchart TD
    T1[Transaction 1: Commit Request] --> Queue[Lock-Free Group Commit Queue]
    T2[Transaction 2: Commit Request] --> Queue
    T3[Transaction 3: Commit Request] --> Queue
    Queue --> Leader[Leader Thread: Claims entire queued batch]
    Leader -->|Single batched write() + fdatasync()| NVMe[Physical NVMe Disk]
    NVMe --> Wake[Leader wakes all follower transactions simultaneously]
```

---

## 1. Pipelining Invariants

1. **Leader-Follower Handshake:** The first thread to reach the commit stage becomes the Group Leader; subsequent transactions register as followers and sleep on futexes.
2. **IOPS Amortization:** Instead of executing $N$ distinct `fdatasync()` calls (each bounded by $100\mu	ext{s}$ storage controller latency), the leader issues a single consolidated flush.
3. **LSN Progression:** The Log Sequence Number (LSN) advances monotonically, ensuring followers commit only after their respective log segment is persisted.
