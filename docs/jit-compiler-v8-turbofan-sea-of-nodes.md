# JIT Compilation: V8 TurboFan Sea-of-Nodes Intermediate Representation

V8 TurboFan represents JavaScript code using a Sea-of-Nodes graph that unifies control-flow, data-flow, and memory effect dependencies into a single relaxed dependency graph.

```mermaid
flowchart TD
    subgraph SeaOfNodes[Sea of Nodes Graph: Unordered Nodes]
        N_Start[Start Node]
        N_Const[Constant 42]
        N_Param[Parameter x]
        N_Add[Speculative NumberAdd Node]
        N_Effect[Effect Chain: Memory State]
        N_Control[Control Chain: Branch Condition]
    end
    N_Const & N_Param -->|Value Edges: Solid Line| N_Add
    N_Start -->|Effect Edge: Dotted Line| N_Effect
    N_Start -->|Control Edge: Bold Line| N_Control
    N_Add --> Schedule[Global Code Motion Scheduler]
    Schedule --> LinearCFG[Linear Machine Code Basic Blocks]
```

---

## 1. Edge Typologies

* **Value Edges (Data Flow):** Carry computed values between nodes without imposing execution sequence constraints.
* **Effect Edges (Memory Flow):** Order mutable memory loads and stores to preserve program semantics without forcing arbitrary non-aliasing instructions into fixed execution orders.
* **Control Edges (Execution Flow):** Explicitly enforce branch divergence (`Branch`, `IfTrue`, `IfFalse`, `Merge`, `Loop`).
* **Late Scheduling:** Nodes float freely until the final instruction scheduling pass binds them into basic blocks based on dominator trees and register pressure models.
