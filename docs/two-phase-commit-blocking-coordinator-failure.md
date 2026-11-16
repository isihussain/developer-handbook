# Distributed Transactions: Two-Phase Commit Coordinator Failure

Two-Phase Commit (2PC) guarantees atomic commitment across distributed participants but suffers from synchronous blocking when the coordinator crashes during Phase 2.

```mermaid
sequenceDiagram
    participant C as Coordinator
    participant P1 as Participant 1
    participant P2 as Participant 2

    C->>P1: Prepare?
    C->>P2: Prepare?
    P1-->>C: Vote: YES (Holding Row Locks)
    P2-->>C: Vote: YES (Holding Row Locks)
    Note over C: Coordinator crashes before writing Commit log!
    Note over P1,P2: BLOCKED! Participants cannot release locks or abort unilaterally!
```

---

## 1. The Uncertainty Period
Once a participant votes `YES`, it enters the uncertain state:
- It cannot abort unilaterally because the coordinator might have decided to commit.
- It cannot commit unilaterally because another participant might have voted `NO`.
