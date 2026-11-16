# Linux Storage: blk-mq Multi-Queue NVMe Architecture

Legacy Linux block layer utilized a single request queue protected by a single lock. `blk-mq` scales to millions of IOPS using two-level multi-queues.

```mermaid
flowchart TD
    CPU0[CPU Core 0] --> SQ0[Software Queue 0 (Per-CPU, Lockless)]
    CPU1[CPU Core 1] --> SQ1[Software Queue 1 (Per-CPU, Lockless)]
    SQ0 --> Map[Queue Mapping Layer]
    SQ1 --> Map
    Map --> HQ0[Hardware Queue 0 (NVMe Controller)]
    Map --> HQ1[Hardware Queue 1 (NVMe Controller)]
    HQ0 --> NVMe[Physical NVMe SSD Flash Arrays]
```

---

## 1. Lockless Ingestion
Submissions occur without cross-core lock contention, matching the hardware parallelism of modern multi-channel PCIe Gen5 NVMe drives.
