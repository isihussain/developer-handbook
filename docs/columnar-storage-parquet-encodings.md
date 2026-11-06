# Apache Parquet Columnar Storage Encodings

Apache Parquet organizes tabular data into column chunks, row groups, and pages with specialized encoding schemes.

```
[ Parquet Row Group Structure ]
+-------------------------------------------------------------+
| Column Chunk 1 (Dictionary + RLE Encoded)                   |
+-------------------------------------------------------------+
| Column Chunk 2 (Delta Binary Packed)                        |
+-------------------------------------------------------------+
| Footer (Metadata, Min/Max statistics, Column Offsets)      |
+-------------------------------------------------------------+
```

---

## 1. Primary Encodings

- **Dictionary Encoding**: Replaces repetitive strings with small integer dictionary IDs.
- **Run-Length Encoding (RLE) & Bit-Packing**: Compresses repeating boolean/definition levels.
- **Delta Binary Packing**: Stores numerical timestamps or IDs as compact incremental deltas.
