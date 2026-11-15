# Compiler Optimization: LLVM ThinLTO Distributed Cross-Module Inlining

ThinLTO combines whole-program Link-Time Optimization (LTO) effectiveness with standard modular compilation scalability by utilizing compact module summaries for cross-module decisions.

```mermaid
flowchart LR
    subgraph Phase1[Phase 1: Clang Compile]
        Src1[Module A.cpp] --> Obj1[A.bc: Bytecode + Summary]
        Src2[Module B.cpp] --> Obj2[B.bc: Bytecode + Summary]
    end
    subgraph Phase2[Phase 2: Thin Linker - Serial / Fast]
        CombinedIndex[Combined Global Index: Call Graph & Inlining Heuristics]
        Obj1 & Obj2 --> CombinedIndex
    end
    subgraph Phase3[Phase 3: Parallel Distributed Backends]
        OptA[Optimize A: Imports hot functions from B]
        OptB[Optimize B: Imports hot functions from A]
        CombinedIndex --> OptA & OptB
    end
```

---

## 1. Operating Phases

1. **Summary Generation:** Compilers generate lightweight summaries (call graphs, referenced symbols, instruction counts) for each translation unit without linking giant monolithic IR graphs.
2. **Thin Link Phase:** Reads summaries into memory in milliseconds, resolves whole-program call graphs, and decides which functions to import across module boundaries.
3. **Parallel Backend Phase:** Independent backend worker threads load only imported bytecode definitions, performing dead-code stripping and cross-module inlining in parallel across all CPU cores.
