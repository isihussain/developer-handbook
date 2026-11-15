# HotSpot C2 JIT Deoptimization & On-Stack Replacement (OSR)

The HotSpot JVM compiles hot bytecode into optimized native machine code assuming speculative class invariants. When class loading invalidates an assumption, deoptimization reconstructs interpreter stack frames on the fly.

```mermaid
sequenceDiagram
    participant JIT as Native JIT Compiled Code
    participant Trap as Uncommon Trap Handler
    participant Interp as Bytecode Interpreter

    JIT->>JIT: Speculative Invariant Check (Class Hierarchy)
    Note over JIT: Invariant Violated!
    JIT->>Trap: Jump to Uncommon Trap Stub
    Trap->>Trap: Read ScopeDesc Debug Info & Reconstruct Local Variables
    Trap->>Interp: Resume execution at next Bytecode BCI (On-Stack Replacement)
```

---

## 1. ScopeDesc Metadata

C2 stores debug metadata mapping machine registers and stack slots back to bytecode interpreter variables for every safepoint, permitting exact state recovery during deoptimization.
