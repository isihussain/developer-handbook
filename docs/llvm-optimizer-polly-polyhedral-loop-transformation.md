# Compiler Optimization: LLVM Polly Polyhedral Loop Transformations

LLVM Polly models nested loop iterations as integer points within multidimensional polyhedra (Z-polyhedra), applying linear matrix transformations to optimize cache locality and parallelism.

```mermaid
flowchart TD
    LoopNest["Nested Loop: for (i=0; i<N; ++i) for (j=0; j<M; ++j)"] --> PolyModel[Polyhedral Model: Integer Polyhedron Domain]
    PolyModel --> Schedule[Affine Scheduling Transformation Matrix]
    Schedule --> Transforms{Polyhedral Transformations}
    Transforms --> Tiling[Loop Tiling: Fits iterations into L1/L2 cache blocks]
    Transforms --> Skewing[Loop Skewing: Eliminates loop-carried dependencies]
    Transforms --> Parallel[SIMD / OpenMP Automatic Vectorization]
```

---

## 1. Polyhedral Representation

* **Iteration Domain ($D$):** System of affine inequalities representing loop bounds:

$$\mathcal{D} = \{ ec{i} \in \mathbb{Z}^n \mid A ec{i} + ec{b} \ge 0 \}$$

* **Access Relations ($R$):** Maps iteration points to target array memory locations.
* **Dependency Polyhedra:** Identifies Read-After-Write (RAW) data hazards. Any matrix transformation $T$ that preserves dependency vector positivity ($T ec{d} > 0$) represents a semantically valid loop restructuring.
