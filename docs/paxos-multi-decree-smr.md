# Multi-Paxos: State Machine Replication Architecture

Multi-Paxos optimizes basic single-decree Paxos for sequences of commands, eliminating Phase 1 (Prepare/Promise) roundtrips during steady-state leadership.

```mermaid
sequenceDiagram
    participant Proposer as Stable Leader (Proposer)
    participant Acceptor1 as Acceptor 1
    participant Acceptor2 as Acceptor 2
    participant Acceptor3 as Acceptor 3
    Note over Proposer,Acceptor3: Steady State: Phase 1 Bypassed
    Proposer->>Acceptor1: Accept(slot=42, term=5, value=V)
    Proposer->>Acceptor2: Accept(slot=42, term=5, value=V)
    Acceptor1-->>Proposer: Accepted(slot=42, term=5)
    Acceptor2-->>Proposer: Accepted(slot=42, term=5)
    Note over Proposer: Majority Reached (2/3) -> Commit
    Proposer->>Acceptor1: Commit(slot=42)
    Proposer->>Acceptor2: Commit(slot=42)
```

---

## 1. Key Invariants

1. **Stable Leader Election:** Proposers negotiate leadership once using Phase 1 across all upcoming slots. Once established, proposals require only Phase 2 (Accept/Accepted).
2. **Log Holes:** Missing slots caused by network partitions or uncommitted ballot proposals are filled with no-op operations to keep sequential log order intact.
3. **Reconfiguration:** Cluster membership shifts (e.g., adding nodes) are serialized as log state machine transitions, taking effect after an offset delay $k$.
