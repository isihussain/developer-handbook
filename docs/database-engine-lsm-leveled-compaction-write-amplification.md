# Storage Engines: LSM-Tree Leveled vs Tiered Compaction

Log-Structured Merge (LSM) storage engines (RocksDB, Cassandra) optimize write throughput by appending data sequentially to SSTables, using Leveled or Tiered compaction to manage storage amplification trade-offs.

```mermaid
flowchart TD
    MemTable[MemTable in RAM] -->|Flush| L0[Level 0: Overlapping Key Ranges]
    subgraph LeveledCompaction[Leveled Compaction (RocksDB)]
        L1[Level 1: Non-overlapping Key Ranges (e.g. 10MB)]
        L2[Level 2: Non-overlapping Key Ranges (e.g. 100MB)]
        L3[Level 3: Non-overlapping Key Ranges (e.g. 1GB)]
        L0 -->|Merge Sort overlapping keys| L1
        L1 -->|Merge Sort overlapping keys| L2
        L2 -->|Merge Sort overlapping keys| L3
    end
```

---

## 1. Amplification Trade-Offs

| Metric | Leveled Compaction | Size-Tiered (STCS) |
|---|---|---|
| **Write Amplification (WAF)** | High ($10	imes - 30	imes$) | Low ($2	imes - 8	imes$) |
| **Read Amplification (RAF)** | Low (Only 1 SSTable per level) | High (Must search all SSTables per tier) |
| **Space Amplification (SAF)** | Low ($pprox 10\% - 20\%$ overhead) | High (Requires up to $50\%$ free disk headroom) |
| **Ideal Workload** | Read-heavy or balanced transactional KV | Write-heavy telemetry / event logs |
