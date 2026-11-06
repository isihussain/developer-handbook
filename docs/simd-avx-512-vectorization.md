# AVX-512 Vectorization Architecture

Intel and AMD AVX-512 expands SIMD execution to 512-bit registers (`ZMM0`..`ZMM31`).

```
[ 512-bit ZMM Register Layout ]
+---------------------------------------------------------------+
|  16 x 32-bit float/int   OR   8 x 64-bit double/int64         |
+---------------------------------------------------------------+
```

---

## 1. Predication via Opmasks (`k0`..`k7`)

Permits per-lane execution without branching:
```c
__m512 va = _mm512_loadu_ps(&a[i]);
__mmask16 mask = _mm512_cmp_ps_mask(va, _mm512_setzero_ps(), _CMP_GT_OQ);
__m512 vc = _mm512_mask_add_ps(va, mask, va, vb);
```
