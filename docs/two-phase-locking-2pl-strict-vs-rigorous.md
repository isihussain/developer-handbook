# Transaction Processing: Two-Phase Locking (2PL) Serializability

Two-Phase Locking guarantees conflict serializability by dividing lock management into an expanding (growing) phase and a shrinking phase.

```mermaid
flowchart LR
    GrowingPhase[Growing Phase: Transactions acquire locks; ZERO locks released] --> ShrinkingPhase[Shrinking Phase: Locks released; ZERO locks acquired]
    subgraph Strict_2PL
        Strict[Strict 2PL: All Exclusive (X) locks held until COMMIT: Prevents Cascading Aborts!]
    end
    subgraph Rigorous_2PL
        Rigorous[Rigorous 2PL: ALL Shared (S) and Exclusive (X) locks held until COMMIT: Enforces Strict Serializability!]
    end
```

---

## 1. Deadlock Inevitability
Under 2PL, deadlocks are mathematically possible. Database engines resolve them via wait-for graphs or lock wait timeouts.
