# JIT Compilation: On-Stack Replacement (OSR) Execution Transitions

On-Stack Replacement (OSR) migrates an actively executing long-running loop from slow interpreter bytecode or baseline tiers into highly optimized native machine code mid-execution.

```mermaid
sequenceDiagram
    participant Interpreter as Bytecode Interpreter Loop
    participant Profiler as Invocation & Backedge Counter
    participant JIT as Background JIT Compiler
    participant Native as Optimized OSR Native Frame
    Interpreter->>Profiler: Increments backedge counter on loop iteration
    Note over Profiler: Threshold exceeded (Hot loop detected!)
    Profiler->>JIT: Triggers background compile with OSR entry point
    JIT-->>Interpreter: Emits compiled OSR code block
    Interpreter->>Native: OSR Migration: Unmarshals local variables into CPU registers
    Note over Native: Continues execution at native CPU clock speeds
```

---

## 1. Frame Reconstruction Invariants

1. **State Reconstruction:** The runtime maps interpreter local variables and operand stack slots directly into CPU registers and stack spill slots designated by the JIT compiler's register allocator.
2. **Loop Entry Invariant:** Execution jumps directly to the loop header instruction within the compiled code body rather than the function prologue.
3. **Deoptimization Mirror:** If speculative invariants fail within the loop, the native frame deoptimizes back to interpreter stack frames using opposite metadata unmarshaling.
