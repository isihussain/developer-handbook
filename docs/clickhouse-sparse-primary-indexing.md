# ClickHouse: Sparse Primary Indexing & MergeTree Engine

Unlike traditional B-Tree databases that index every row, ClickHouse MergeTree tables use sparse indexing with granular marks to support petabyte-scale analytical queries.

```mermaid
flowchart LR
    subgraph PrimaryIndex[Primary Index in RAM: 1 entry per 8192 rows]
        I0["Index 0: User 1000"] --> I1["Index 1: User 9500"] --> I2["Index 2: User 18000"]
    end
    subgraph ColumnData[Disk Column Files .bin]
        M0[Granule 0: Rows 0..8191]
        M1[Granule 1: Rows 8192..16383]
        M2[Granule 2: Rows 16384..24575]
    end
    I0 -.-> M0
    I1 -.-> M1
    I2 -.-> M2
```

---

## 1. Key Invariants

1. **Granule Sizing:** Default index granularity is 8192 rows. The primary index stores one entry per granule, fitting hundreds of millions of rows into a few megabytes of RAM.
2. **Skipping Indexes:** Secondary min-max, set, and Bloom filter indexes discard irrelevant granules before fetching column data files from disk.
3. **Sorting Key Selection:** Columns with lower cardinality must precede high-cardinality keys in the `ORDER BY` tuple to maximize compression and range pruning.
