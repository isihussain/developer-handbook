# Data Engineering: Apache Parquet Physical Layout Architecture

Apache Parquet is an open-source columnar storage format designed for efficient analytical query execution with predicate pushdown.

```mermaid
flowchart TD
    File[Parquet File] --> RG1[Row Group 1: 512MB-1GB chunk]
    File --> Footer[File Metadata & Footer: Schema & Statistics]
    RG1 --> CC1[Column Chunk 1: 'timestamp']
    RG1 --> CC2[Column Chunk 2: 'user_id']
    CC2 --> P1[Data Page 1: Min/Max Index Header]
    CC2 --> P2[Data Page 2: Min/Max Index Header]
```

---

## 1. Predicate Pushdown Mechanics
Query engines (DuckDB, Trino, Spark) inspect the file footer before reading column data:
- If `user_id > 1000` and Page 1 metadata indicates `Max = 999`, the entire Data Page is skipped without I/O.
