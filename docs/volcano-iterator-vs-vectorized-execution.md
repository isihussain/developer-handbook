# Query Engines: Volcano Iterator vs Vectorized Execution

Modern SQL analytical engines replace the classic tuple-at-a-time Volcano Iterator model with vectorized block-oriented processing to exploit CPU L1 data cache and SIMD execution units.

```mermaid
flowchart TD
    subgraph Volcano[Volcano Iterator Model: Tuple-at-a-time]
        V_Scan[SeqScan.next] -->|1 tuple| V_Filter[Filter.next]
        V_Filter -->|1 tuple| V_Agg[Aggregate.next]
    end
    subgraph Vectorized[Vectorized Execution Model: Block-at-a-time]
        Vec_Scan[VectorScan.fetch] -->|Batch: 2048 vectors| Vec_Filter[SIMD Predicate Filter]
        Vec_Filter -->|Selection Vector| Vec_Agg[SIMD Hash Aggregate]
    end
```

---

## 1. Performance Differences

| Dimension | Volcano Iterator Model | Vectorized Engine (DuckDB/ClickHouse) |
|---|---|---|
| **Call Overhead** | High: Dynamic virtual dispatch per tuple | Low: Loop iteration over columnar arrays |
| **CPU Cache Efficiency** | Poor: Interleaved row attributes evict L1 | High: Dense columnar vectors fit L1/L2 caches |
| **SIMD Exploitation** | None | Native: AVX-512 / ARM Neon vector instructions |
| **Branch Mispredictions**| Frequent inside per-tuple predicates | Minimal across batched loop kernels |
