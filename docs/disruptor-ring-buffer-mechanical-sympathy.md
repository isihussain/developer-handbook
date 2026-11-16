# High-Frequency Trading: LMAX Disruptor Mechanical Sympathy

The LMAX Disruptor eliminates queue lock contention and cache invalidation churn by using pre-allocated circular ring buffers and memory-sequenced barriers.

```mermaid
flowchart TD
    Producer1[Publisher Thread] --> RingBuffer[Pre-Allocated RingBuffer: Zero Runtime GC Allocation!]
    RingBuffer --> SequenceBarrier[Sequence Barrier]
    SequenceBarrier --> Consumer1[Journaling Handler: Core 1]
    SequenceBarrier --> Consumer2[Replication Handler: Core 2]
    Consumer1 --> BusinessLogic[Business Logic Engine: Core 3 (Processes 6 million ops/sec!)]
    Consumer2 --> BusinessLogic
```

---

## 1. Mechanical Sympathy Principles
1. **Zero Garbage Collection:** Entries are pre-allocated at startup; publishers overwrite mutable slots.
2. **False Sharing Elimination:** Sequence numbers are padded with 7 unused 64-bit integers (56 bytes + 8 bytes = 64 bytes) to fill an entire L1 cacheline.
