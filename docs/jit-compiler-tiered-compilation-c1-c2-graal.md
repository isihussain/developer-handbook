# JVM Internals: Tiered Compilation Architecture (C1, C2 & Graal)

HotSpot JVM manages compilation across five tiered execution levels, balancing rapid application startup with maximum steady-state peak machine code optimization.

```mermaid
stateDiagram-v2
    [*] --> Level0: Application Launch
    Level0 --> Level3: Method Invocation Counter Threshold
    Level3 --> Level4: High Invocation Frequency (Hot Code)
    Level4 --> Level0: Deoptimization (Speculation failure)
    state Level0 { Interpreter: Pure bytecode interpretation }
    state Level1 { C1_NoProfile: Fast JIT compilation without profiling }
    state Level2 { C1_BasicProfile: JIT compilation with lightweight counters }
    state Level3 { C1_FullProfile: JIT compilation with MDO branch profiling }
    state Level4 { C2_Graal: High-Tier optimizing compiler, Escape Analysis }
```

---

## 1. Tier Characteristics

* **Level 0 (Interpreter):** Generates zero native code; records invocation and loop backedge frequencies.
* **Level 3 (C1 Full Profiling):** Generates fast machine code containing MethodDataObjects (MDO) that record observed branch probabilities and type feedback vectors.
* **Level 4 (C2 / Graal):** Uses MDO profiles to perform global optimizations: speculative devirtualization, escape analysis, loop unrolling, and vectorization.
