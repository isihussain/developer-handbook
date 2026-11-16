# CPU Microarchitecture: SIMD AVX-512 vs ARM NEON

Single Instruction, Multiple Data (SIMD) vector registers allow a single CPU clock cycle to execute parallel arithmetic across multiple data elements.

```mermaid
flowchart TD
    subgraph AVX512[AVX-512: 512-bit ZMM Register]
        Int32_1[32-bit int]
        Int32_2[32-bit int]
        Dots[...]
        Int32_16[32-bit int: 16 integers processed simultaneously per instruction!]
    end
    AVX512 --> FMA[VFMADD213PS: Fused Multiply-Add in 1 CPU Cycle!]
```

---

## 1. Data Alignment Invariant
Unaligned vector memory loads across 64-byte cache line boundaries incur substantial performance penalties. High-performance code enforces 64-byte alignment (`posix_memalign` / `_aligned_malloc`).
