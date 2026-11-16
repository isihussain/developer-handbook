# Storage Engines: Columnar Dictionary Encoding and Bit-Packing

Column-oriented analytical databases (ClickHouse, Snowflake, DuckDB) compress repeating values into compact surrogate keys using dictionary tables and integer bit-packing.

```mermaid
flowchart TD
    RawStrings["Raw Column: ['Chrome', 'Firefox', 'Chrome', 'Safari', 'Chrome']"]
    RawStrings --> Dict["Dictionary: {0: 'Chrome', 1: 'Firefox', 2: 'Safari'}"]
    RawStrings --> Encoded["Surrogate IDs: [0, 1, 0, 2, 0]"]
    Encoded --> BitPack["Bit-Packing: 3 distinct values -> 2 bits per integer!"]
```

---

## 1. Vectorized Execution Advantage
Compressed 2-bit values can be unpacked in batches using SIMD AVX-512 vector instructions, processing billions of rows per second in L1 cache.
