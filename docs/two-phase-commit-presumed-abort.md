# Two-Phase Commit (2PC): Presumed Abort Optimization

Two-Phase Commit guarantees atomic transactions across independent database shards. The Presumed Abort optimization eliminates log flush latency for aborted distributed transactions.

```mermaid
sequenceDiagram
    participant C as Coordinator
    participant P1 as Participant 1
    participant P2 as Participant 2
    Note over C,P2: Phase 1: Prepare
    C->>P1: Prepare(tx_id)
    C->>P2: Prepare(tx_id)
    P1-->>C: Vote_Commit
    P2-->>C: Vote_Abort
    Note over C: Presumed Abort: No log sync needed for abort decision
    C->>P1: Abort(tx_id)
    C->>P2: Abort(tx_id)
    P1-->>C: Ack_Abort
```

---

## 1. Presumed Abort Mechanics

1. **Information Asymmetry:** The coordinator assumes any unknown transaction aborted without writing an explicit abort record to durable WAL.
2. **Cost Reduction:** If any participant votes abort or fails to respond within timeout, the coordinator discards transaction state from memory without issuing an `fsync`.
3. **Commit Barrier:** A commit decision still requires synchronous log flush on the coordinator before issuing commit messages to participants.
