# Database Internals: WAL Group Commit and fsync Pipelining

A single mechanical disk or SSD can only execute a limited number of `fsync()` operations per second. Group commit batches multiple concurrent transactions into a single flush.

```mermaid
sequenceDiagram
    participant Tx1 as Worker Thread 1
    participant Tx2 as Worker Thread 2
    participant Tx3 as Worker Thread 3
    participant Leader as Group Commit Leader
    participant Disk as NVMe / SSD Storage

    Tx1->>Leader: Enqueue Log Record 1 (Becomes Leader)
    Tx2->>Leader: Enqueue Log Record 2
    Tx3->>Leader: Enqueue Log Record 3
    Note over Leader: Leader acquires lock, executes single fsync()
    Leader->>Disk: writev() + fdatasync()
    Disk-->>Leader: Flush Completed
    Leader-->>Tx1: Commit Success
    Leader-->>Tx2: Commit Success
    Leader-->>Tx3: Commit Success
```

---

## 1. Performance Impact
Without group commit: Throughput capped at disk IOPS ($IOPS = 1 / \text{latency}$).  
With group commit: Throughput scales with queue concurrency, amortizing latency across $N$ workers.
