# V8 TurboFan Sea-of-Nodes Speculative Optimization

V8 TurboFan represents JavaScript execution as a unified graph combining data flow, control flow, and side-effect chains without rigid basic block boundaries, enabling aggressive global value numbering and dead code elimination.

```mermaid
flowchart TD
    Ignition[Ignition Bytecode + Type Feedback Vector] --> BytecodeGraph[BytecodeGraphBuilder]
    BytecodeGraph --> SeaOfNodes[Sea-of-Nodes IR Representation]
    SeaOfNodes --> SpeculativeOpt[Speculative Typing & Optimization]
    SpeculativeOpt --> EscapeAnalysis[Escape Analysis & Scalar Replacement]
    EscapeAnalysis --> Scheduling[Instruction Scheduling & Register Allocation]
    Scheduling --> MachineCode[Native Machine Code Generation]
```

---

## 1. Edge Categories in Sea-of-Nodes

* **Value Edges:** Carry computational data results between operator nodes.
* **Effect Edges:** Track sequencing of stateful operations (memory writes, object mutations).
* **Control Edges:** Direct branches, conditional splits, and merge points.
