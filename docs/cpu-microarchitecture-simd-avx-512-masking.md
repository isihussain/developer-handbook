# CPU Microarchitecture: Intel AVX-512 Opmask Registers & Conditional SIMD

AVX-512 introduces 8 dedicated opmask registers (`%k0` through `%k7`) that govern individual vector lane execution, enabling branchless SIMD evaluation of conditional algorithms.

```mermaid
flowchart TD
    VectorA[ZMM1: 16 x 32-bit Integers] --> Compare[Vector Comparison: vpcmpgtd]
    VectorB[ZMM2: 16 x 32-bit Integers] --> Compare
    Compare --> Opmask[Opmask Register %k1: 16-bit Mask e.g. 1010_1100_0001_1011]
    Opmask --> Blending[Vector Blending: vpaddd ZMM3 {%k1}{z}, ZMM1, ZMM2]
    Blending --> Result[Target ZMM3: Lanes with mask 0 are ZEROED {z} or MERGED]
```

---

## 1. Zeroing vs Merging Masking

* **Merging Masking (`{k1}`):** Unmasked destination vector lanes retain their previous values unchanged.
* **Zeroing Masking (`{k1}{z}`):** Unmasked destination vector lanes are automatically set to zero, eliminating artificial register read dependencies on previous state.
* **Mask Operations:** Opmask registers operate via dedicated bitwise instructions (`kandw`, `korw`, `kxnorw`, `knotw`), enabling conditional evaluation without instruction branch mispredictions.
