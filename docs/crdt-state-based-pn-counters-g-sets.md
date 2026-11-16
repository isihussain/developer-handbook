# Distributed State: State-Based CRDTs (PN-Counters & G-Sets)

Conflict-Free Replicated Data Types (CRDTs) guarantee Strong Eventual Consistency (SEC) by using join-semilattices where the merge operation is Commutative, Associative, and Idempotent.

```mermaid
flowchart LR
    subgraph Node_A[Replica A]
        P_A[P: [A:3, B:0]]
        N_A[N: [A:1, B:0]]
    end
    subgraph Node_B[Replica B]
        P_B[P: [A:1, B:4]]
        N_B[N: [A:0, B:2]]
    end
    Node_A -->|Merge: max P, max N| Merged[P: A:3, B:4 | N: A:1, B:2]
    Node_B -->|Merge: max P, max N| Merged
    Merged --> Value["Computed Value = sum(P) - sum(N) = 7 - 3 = 4"]
```

---

## 1. Mathematical Invariants
- **Commutative:** $A \sqcup B = B \sqcup A$
- **Associative:** $(A \sqcup B) \sqcup C = A \sqcup (B \sqcup C)$
- **Idempotent:** $A \sqcup A = A$
