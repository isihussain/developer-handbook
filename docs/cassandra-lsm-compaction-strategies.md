# Apache Cassandra: LSM-Tree Compaction Strategies

Cassandra persists row mutations to disk in immutable Sorted String Tables (SSTables). Compaction merges obsolete row versions to control read amplification, disk amplification, and tombstone accumulation.

```mermaid
flowchart TD
    subgraph STCS: Size-Tiered
        ST1[Small SSTables of similar size] -->|Triggered when 4 SSTables match size| MergeST[Merge into larger SSTable]
    end
    subgraph LCS: Leveled Compaction
        L0[Level 0: Overlapping Keys] --> L1[Level 1: 10MB Non-overlapping]
        L1 --> L2[Level 2: 100MB Non-overlapping]
    end
    subgraph TWCS: Time-Window Compaction
        TW1[Time Bucket 2026-11-01]
        TW2[Time Bucket 2026-11-02]
    end
```

---

## 1. Strategy Comparison Matrix

| Strategy | Best Use Case | Read Amplification | Write Amplification | Space Overhead |
|---|---|---|---|---|
| **STCS** | Write-heavy, non-updating workloads. | High | Low | High (up to 50% free space required) |
| **LCS** | Read-heavy, frequent update workloads. | Very Low | High | Low (~10% free space required) |
| **TWCS** | Time-series data with TTL expiration. | Low | Minimal | Minimal |
