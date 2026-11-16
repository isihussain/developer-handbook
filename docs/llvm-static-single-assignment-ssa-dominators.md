# Compiler Engineering: LLVM SSA Form and Dominance Frontiers

In Static Single Assignment (SSA) form, every variable is assigned exactly once. When control flow branches merge, $\phi$ (Phi) nodes select the value based on the predecessor basic block.

```mermaid
flowchart TD
    Entry[BB_Entry: x_0 = 5] --> Branch{Condition}
    Branch -->|True| BB1[BB_True: x_1 = 10]
    Branch -->|False| BB2[BB_False: x_2 = 20]
    BB1 --> Merge[BB_Merge]
    BB2 --> Merge
    Merge --> Phi["x_3 = phi( [x_1, BB_True], [x_2, BB_False] )"]
```

---

## 1. Optimization Power
SSA form dramatically simplifies compiler optimization passes:
- Dead Code Elimination (DCE)
- Sparse Conditional Constant Propagation (SCCP)
- Global Value Numbering (GVN)
