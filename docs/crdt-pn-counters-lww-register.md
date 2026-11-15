# CRDT Architecture: PN-Counters & LWW-Element-Set

Conflict-Free Replicated Data Types guarantee eventual consistency across network partitions without distributed lock coordination by utilizing join-semilattice algebraic properties.

```mermaid
flowchart TD
    subgraph Replica 1
        P1[Positive Vector: P_1]
        N1[Negative Vector: N_1]
    end
    subgraph Replica 2
        P2[Positive Vector: P_2]
        N2[Negative Vector: N_2]
    end
    P1 <-->|State Synchronization| P2
    N1 <-->|State Synchronization| N2
    Merge[Merge Operation: P_merged = max P1, P2 ; N_merged = max N1, N2]
    Value[Counter Value = sum P_merged - sum N_merged]
```

---

## 1. Join-Semilattice Properties

Any state-based (CvRDT) merge operator $\sqcup$ must satisfy:
1. **Commutativity:** $x \sqcup y = y \sqcup x$
2. **Associativity:** $(x \sqcup y) \sqcup z = x \sqcup (y \sqcup z)$
3. **Idempotence:** $x \sqcup x = x$
