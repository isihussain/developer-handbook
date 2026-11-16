# Hardware Concurrency: x86-64 TSO vs ARM/RISC-V Weak Memory Models

Different CPU architectures provide different memory ordering guarantees, dictating how aggressively compilers must emit hardware fence instructions.

```mermaid
flowchart TD
    Architecture -->|x86-64: Strong TSO| TSO[Total Store Order: Only Store-Load can be reordered! Loads & Stores are mostly ordered]
    Architecture -->|ARM / RISC-V: Weak| Weak[Weakly Ordered: Store-Store, Load-Load, Load-Store can ALL be reordered!]
    Weak --> Barriers[Requires explicit DMB (Data Memory Barrier) / fence instructions]
```

---

## 1. Cross-Platform Pitfall
Lock-free C++ code with missing memory barriers might run flawlessly on x86-64 due to hardware TSO, but crash under race conditions when compiled on ARM64 (Apple Silicon / AWS Graviton).
