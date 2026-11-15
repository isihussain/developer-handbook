# Compiler Optimization: LLVM Loop Vectorizer & SLP Vectorization

LLVM maximizes SIMD throughput using two complementary vectorizers: the Loop Vectorizer (widening loop bodies) and the Superword-Level Parallelism (SLP) Vectorizer (combining isomorphic scalar instruction trees).

```mermaid
flowchart TD
    subgraph LoopVectorizer[Loop Vectorizer: Iteration Widening]
        ScalarLoop["for (i=0; i<N; ++i) a[i] = b[i] + c[i];"] --> CostModel{Vector Factor VF Cost}
        CostModel --> VectorLoop["for (i=0; i<N; i+=8) v_a = _mm256_add_ps(v_b, v_c);"]
    end
    subgraph SLP_Vectorizer[SLP Vectorizer: Straight-Line Code]
        ScalarTree["x = a + b; y = c + d; z = e + f; w = g + h;"] --> SLP_Tree[Bundles parallel operations]
        SLP_Tree --> SIMD_Inst["v_res = _mm128_add_epi32(v1, v2);"]
    end
```

---

## 1. Vectorization Prerequisites

* **Trip Count & SCEV:** Scalar Evolution (SCEV) calculates exact or minimum trip counts to construct safe vectorized loops alongside scalar cleanup tails.
* **Alias Analysis:** Memory references must be proven independent or guarded with runtime pointer checks (`ptr_a + len < ptr_b`).
* **Reduction Detection:** Recognizes scalar accumulators (`sum += arr[i]`) and transforms them into horizontal vector add reductions.
