# Database Query Execution: Volcano Iterator Model

Classic query execution pipeline where relational operators implement an iterator interface (`open()`, `next()`, `close()`).

---

## 1. Vectorized Execution (Bratbergsengen / MonetDB)

Modern analytical engines (DuckDB, ClickHouse) replace tuple-at-a-time processing with batch processing vectors of 1024 values per `next()` call, maximizing CPU L1 instruction cache efficiency.
