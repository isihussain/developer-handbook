# Compiler Optimization: LLVM Global Value Numbering (GVN) & PRE

LLVM GVN maps semantically equivalent expressions to unique integer value numbers, enabling Partial Redundancy Elimination (PRE) to hoist redundant computations out of conditional control-flow paths.

```mermaid
flowchart TD
    BB_Entry[BasicBlock Entry: a = x + y] --> CondBranch{Branch Condition}
    CondBranch -->|Path Left| BB_Left[BasicBlock Left: Path without recomputation]
    CondBranch -->|Path Right| BB_Right[BasicBlock Right: b = x + y (Redundant!)]
    BB_Left --> BB_Merge[Merge Block: phi(a, b)]
    BB_Right --> BB_Merge
    subgraph PRE_Hoisted[After PRE Optimization]
        BB_Entry_New[BasicBlock Entry: t = x + y hoisted once] --> CondBranch_New{Branch Condition}
    end
```

---

## 1. GVN Value Table Construction

1. **Expression Hashing:** Canonicalizes commutative operands ($y + x 	o x + y$) and computes lookup hashes across basic block dominance trees.
2. **Congruence Classes:** Expressions with matching operand value numbers belong to identical congruence classes and share a single representative SSA value.
3. **PRE Insertion:** If an expression is partially available on only a subset of incoming control paths, PRE inserts the computation on remaining paths and replaces original instructions with a $\phi$-node.
