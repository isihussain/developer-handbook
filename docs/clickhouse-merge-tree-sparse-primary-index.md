# Columnar Analytics: ClickHouse MergeTree Sparse Index Architecture

Unlike traditional B-Trees that index every single row, ClickHouse stores one primary index entry for every 8,192 rows (one index mark).

```mermaid
flowchart TD
    Query["SELECT * WHERE user_id = 45000"] --> SparseIndex[Sparse Primary Index: Fully resident in RAM]
    SparseIndex --> MarkLookup["Mark 5: Covers rows [40960 to 49151]"]
    MarkLookup --> ReadCompressed[Read and decompress ONLY Column Compressed Blocks for Mark 5]
    ReadCompressed --> FilterRows[Filter 8192 rows via SIMD vector scans]
```

---

## 1. RAM Footprint
A ClickHouse table containing 10 billion rows has only ~1.2 million index marks, fitting its entire primary index inside just a few megabytes of RAM.
