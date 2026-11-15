# Compiler Optimization: Post-Link Binary Optimization with BOLT & PGO

Facebook BOLT (Binary Optimization and Layout Tool) analyzes execution sample profiles (Linux `perf`) to reorder basic blocks and functions in compiled ELF binaries, optimizing instruction cache hit rates.

```mermaid
flowchart TD
    Binary[Compiled ELF Binary] --> Profile[Run under Production Load: perf record -b]
    Profile --> BOLT[BOLT Binary Optimizer]
    subgraph BOLT_Passes[Post-Link Layout Optimization]
        BOLT --> BlockReorder[Basic Block Reordering: Moves hot fall-through blocks together]
        BOLT --> FuncReorder[Function Placement: Groups hot functions into contiguous 2MB pages]
        BOLT --> SplitCold[Hot / Cold Code Splitting: Moves error paths out-of-line]
    end
    BOLT_Passes --> OptimizedELF[Optimized ELF Binary: I-TLB & I-Cache Misses Reduced 30%]
```

---

## 1. Microarchitectural Impact

1. **Branch Misprediction Reduction:** Hot branches are converted to fall-through layout paths, avoiding branch instructions completely.
2. **I-Cache Efficiency:** Eliminates cold instructions (error handling, debug assertions) from L1 instruction cache lines.
3. **Hugepage Alignment:** Groups all hot functions together into contiguous 2MB memory blocks, slashing Instruction TLB (iTLB) misses.
