# Time-Series Architecture: TimescaleDB Hypertables & Chunk Pruning

TimescaleDB partitions PostgreSQL tables across time dimensions into small, manageable standard tables called **Chunks**.

```mermaid
flowchart TD
    Hypertable[Hypertable: 'sensor_metrics'] --> Chunks[Partitioned into 1-Day Chunks]
    Chunks --> Chunk1["Chunk: 2026-11-14"]
    Chunks --> Chunk2["Chunk: 2026-11-15"]
    Chunks --> Chunk3["Chunk: 2026-11-16 (Active)"]
    Query["SELECT * WHERE time > '2026-11-16 00:00'"] --> Pruner[Constraint Exclusion / Chunk Pruning]
    Pruner --> ReadOnly3[Reads ONLY Chunk 3: Completely skips Chunks 1 and 2!]
```

---

## 1. Memory Residency Advantage
Each active chunk's B-Tree indexes easily fit within the PostgreSQL `shared_buffers` cache, preventing random disk page evictions during high-throughput ingestion.
