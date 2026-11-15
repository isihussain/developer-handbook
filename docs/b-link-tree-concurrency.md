# B-Link Tree: High-Concurrency Indexing Internals

The Lehman-Yao B-Link Tree decouples node splits from top-down tree locking by linking sibling nodes at each level, enabling lock-free and latch-free concurrent reader traversal.

```mermaid
flowchart LR
    subgraph Parent[Parent Level]
        P[Node P: Keys | Children]
    end
    subgraph Level1[Leaf / Internal Level]
        A[Node A: Keys [1..50]] -->|Right Sibling Pointer: High Key 50| B[Node B: Keys [51..100]]
    end
    Parent --> A
    Parent --> B
```

---

## 1. Traversal & Split Invariants

1. **Right-Link Traversal:** Readers descend without locks on parent nodes. If a reader searches for key $K$ in Node $A$ and finds $K > \text{HighKey}(A)$, it follows the right-sibling link to Node $B$ without restarting at root.
2. **Single-Phase Splits:** Writers allocate a new right sibling, copy the upper half of keys, update sibling pointers with atomic memory operations, and link the node before propagating pointers up to the parent.
3. **Deadlock Immunity:** Traversal proceeds strictly left-to-right and top-to-bottom. No thread holds upward lock chains.
