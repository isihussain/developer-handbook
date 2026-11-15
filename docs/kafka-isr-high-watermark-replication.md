# Apache Kafka: In-Sync Replicas (ISR) & High Watermark Architecture

Kafka guarantees zero message loss through partitioned commit logs coordinated by an in-sync replica (ISR) set, exposing messages to consumers only when they advance past the High Watermark (HW).

```mermaid
flowchart LR
    Leader[Partition Leader: Log End Offset 120] -->|Replicates message| Follower1[ISR Follower 1: LEO 120]
    Leader -->|Replicates message| Follower2[ISR Follower 2: LEO 120]
    Leader --> HW[High Watermark HW: 120]
    Lagging[Slow Follower: LEO 85] -.->|Excluded from ISR if replica.lag.time.max.ms exceeded| OutOfSync[Out of Sync]
    HW --> Consumer[Consumers only read up to HW]
```

---

## 1. High Watermark Invariants

* **Log End Offset (LEO):** Offset of the next record to be written in a partition log.
* **High Watermark (HW):** Minimum LEO among all current ISR members; dictates the boundary of committed data visible to consumers.
