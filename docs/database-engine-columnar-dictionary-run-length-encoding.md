# Columnar Storage: Dictionary & Run-Length Encoding (RLE)

Columnar execution engines (Apache Parquet, ClickHouse, DuckDB) compress sorted data columns by combining fixed-length Dictionary Encoding with Run-Length Encoding.

```mermaid
flowchart TD
    RawCol[Raw Text Column: USA, USA, USA, CAN, CAN, MEX] --> Dict[Dictionary Construction]
    Dict --> DictTable["Dictionary Table: 0: USA, 1: CAN, 2: MEX"]
    Dict --> Indices["Integer Stream: 0, 0, 0, 1, 1, 2"]
    Indices --> RLE[Run-Length Encoding Engine]
    RLE --> Compressed["Compressed Output: (0, run=3), (1, run=2), (2, run=1)"]
```

---

## 1. Operating Metrics & SIMD Acceleration

* **Bit-Packing:** Dictionary indices are bit-packed to the minimum bitwidth required ($\lceil \log_2 N ceil$ bits, e.g., 2 bits for 4 distinct values).
* **Predicate Pushdown:** Filter queries (`WHERE country = 'USA'`) compare against the dictionary entry once (`id == 0`), evaluating remaining blocks with SIMD vector equality comparisons directly on packed indices without string decoding.
