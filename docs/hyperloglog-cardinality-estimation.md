# HyperLogLog (HLL) Cardinality Estimation

HyperLogLog (Flajolet et al.) estimates distinct item counts (cardinality) across billions of elements using fixed memory ($<1.5	ext{ KB}$).

```
[ 64-bit Hash Value ]
+---------------------+---------------------------------------+
|  p bits (Register)  |  64 - p bits (Leading Zero Count)    |
+---------------------+---------------------------------------+
```

---

## 1. Harmonic Mean Estimate

$$E = lpha_m m^2 \left( \sum_{j=1}^m 2^{-M[j]} ight)^{-1}$$
- $m = 2^p$ registers.
- $M[j]$ stores maximum count of leading zeroes observed in register $j$.
- Harmonic mean dampens outlier variance. Standard error is approximately $rac{1.04}{\sqrt{m}}$.
