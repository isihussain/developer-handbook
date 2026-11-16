# OLAP Query Engines: ClickHouse Vectorized SIMD Filter Masks

Traditional row-based databases evaluate filter predicates (`WHERE age > 18`) row-by-row via virtual function calls. ClickHouse generates compact 8-bit boolean byte vectors.

```mermaid
flowchart TD
    ColumnArray["Input Array: [15, 22, 34, 12, 45, 18, 25, 9]"] --> SIMD_CMP["AVX2 _mm256_cmpgt_epi32: Vectorized comparison in 1 cycle!"]
    SIMD_CMP --> FilterMask["Filter Mask: [0, 1, 1, 0, 1, 0, 1, 0]"]
    FilterMask --> FilterData["SIMD Filter Copy: Extracts only matching values into output block!"]
```

---

## 1. Zero Branch Mispredictions
Eliminates branching instructions (`if / else`) completely from the inner loop, saturating CPU instruction pipelines at line rate.
