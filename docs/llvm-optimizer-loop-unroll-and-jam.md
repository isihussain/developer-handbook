# Compiler Optimization: LLVM Loop Unroll-and-Jam

Loop Unroll-and-Jam unrolls an outer loop by a factor $K$ and fuses (jams) the resulting duplicate inner loop bodies together, maximizing CPU register reuse across multi-dimensional arrays.

```mermaid
flowchart TD
    subgraph Original[Original Nested Loops]
        Outer["for (i = 0; i < N; ++i)"] --> Inner["for (j = 0; j < M; ++j) a[i][j] = b[i][j] + c[i];"]
    end
    Original --> UnrollJamPass[LLVM LoopUnrollAndJamPass]
    subgraph Jammed[Unrolled and Jammed: Factor 2]
        JammedLoop["for (i = 0; i < N; i += 2) {
    for (j = 0; j < M; ++j) {
        a[i][j] = b[i][j] + c[i];
        a[i+1][j] = b[i+1][j] + c[i+1];
    }
}"]
    end
    UnrollJamPass --> Jammed
```

---

## 1. Optimization Benefits

* **Register Reuse:** Values dependent on outer loop indices (e.g., array pointers, constants) stay pinned in hardware registers across the combined inner loop iterations.
* **Safety Invariant:** Legal only if no backward loop-carried data dependencies exist between outer and inner loop bodies.
