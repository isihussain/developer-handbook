# Apache Parquet Bloom Filter Predicate Pushdown

Eliminates unnecessary disk and network I/O by evaluating split-block Bloom filters stored in column chunk footers.

---

## 1. Query Engine Pushdown

When querying `WHERE uuid = 'xyz'`:
1. Query engine reads 2KB Bloom filter from Parquet file footer.
2. If filter returns 0 (definite negative), skips reading and decompressing the multi-megabyte row group.
