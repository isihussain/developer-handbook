# Memory Ordering: x86 Total Store Order (TSO) vs ARM64 Weak Ordering

Processor architectures enforce different hardware memory consistency models, dictating when memory barriers and acquire/release fences are mandatory.

```mermaid
flowchart TD
    subgraph x86_TSO[x86-64: Total Store Order - Strict]
        TSO1[Reads NOT reordered with Reads]
        TSO2[Writes NOT reordered with Writes]
        TSO3[Reads CAN be reordered before older Writes: Store Buffer bypass]
    end
    subgraph ARM_Weak[ARM64: Weakly Ordered Model - Relaxed]
        ARM1[Reads can be reordered with Reads]
        ARM2[Writes can be reordered with Writes]
        ARM3[Hardware Memory Barriers required: DMB, DSB, ISB]
    end
```

---

## 1. Hardware Barriers

| Architecture | Load-Load Reorder | Store-Store Reorder | Barrier Instruction |
|---|---|---|---|
| **x86-64** | No | No | `mfence` (prevents Store-Load bypass) |
| **ARM64** | Allowed | Allowed | `dmb ishld` (Load barrier), `dmb ish` (Full) |
