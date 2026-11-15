# Distributed Consensus: Multi-Decree Paxos & Stable Leader Optimization

Multi-Decree Paxos sequences an infinite stream of state machine commands across numbered consensus slots, bypassing Phase 1 (Prepare/Promise) when a stable leader is active to achieve 1-RTT consensus.

```mermaid
sequenceDiagram
    participant Client
    participant Leader as Stable Paxos Leader (Ballot b)
    participant Acceptor1 as Acceptor 1
    participant Acceptor2 as Acceptor 2
    Note over Leader,Acceptor2: Phase 1 (Prepare/Promise) executed ONCE for ALL future slots
    Client->>Leader: Command c for Slot i
    Leader->>Acceptor1: Phase 2a: Accept(Slot i, Ballot b, Command c)
    Leader->>Acceptor2: Phase 2a: Accept(Slot i, Ballot b, Command c)
    Acceptor1->>Leader: Phase 2b: Accepted(Slot i, Ballot b)
    Acceptor2->>Leader: Phase 2b: Accepted(Slot i, Ballot b)
    Note over Leader: Quorum reached (2/3) -> Commits Slot i
    Leader-->>Client: Success Acknowledgment
```

---

## 1. Slot Allocation & Gaps

* **Slot Allocation:** The leader allocates monotonically increasing integer slot indices ($1, 2, 3, \dots$) for incoming client commands.
* **Slot Holes:** If a leader fails mid-proposal, the successor leader issues Phase 1 across all uncommitted slots; any slot with no accepted value is filled with a `NOP` command to advance the continuous state machine stream.
