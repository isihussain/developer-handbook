# Database Engines: Cardinality Estimation with HyperLogLog (HLL)

HyperLogLog (HLL) estimates the number of distinct elements (cardinality) in massive datasets using $O(\log \log N)$ bits of memory, providing query optimizers with fast join size predictions.

```mermaid
flowchart TD
    DataVal[Data Element x] --> Hash[64-bit Uniform Hash: h = hash(x)]
    Hash --> Split[Split Hash Bits]
    Split --> Bucket["First p bits (e.g. p=14): Selects 1 of m = 2^p registers"]
    Split --> Run["Remaining bits: Count leading zeros rho(w) + 1"]
    Bucket & Run --> Registers[Register Array: M[j] = max(M[j], rho(w)+1)]
    Registers --> Aggregation[Harmonic Mean Estimation with Bias Correction]
```

---

## 1. Harmonic Mean & Error Bounds

$$E = lpha_m \cdot m^2 \cdot \left(\sum_{j=1}^{m} 2^{-M[j]}ight)^{-1}$$

* **Standard Error:** Relative estimation error is bounded by:

$$	ext{SE} pprox rac{1.04}{\sqrt{m}}$$

* **Memory Footprint:** With $m = 16,384$ registers ($p = 14$) and 6-bit register widths, HLL achieves a $\pm 0.81\%$ standard error across billions of unique values using strictly 12KB of RAM.
