# Google Spanner: TrueTime API and Uncertainty Wait

Google Spanner achieves external consistency (linearizability) across global clusters without communication between read transactions and write transactions by using TrueTime: `TT.now() = [earliest, latest]` with bound `epsilon`.

```mermaid
sequenceDiagram
    participant Tx as Transaction Coordinator
    participant TT as TrueTime Engine (GPS + Atomic Clocks)
    participant Storage as Multi-Version Storage Engine

    Tx->>TT: TT.now() -> [t.earliest, t.latest]
    Note over Tx: Choose commit timestamp s >= t.latest
    Tx->>Storage: Write mutations with timestamp s
    Note over Tx: Wait-Out Uncertainty: Sleep until TT.now().earliest > s
    Tx-->>Client: Transaction Committed!
```

---

## 1. TrueTime Invariant
For any two transactions $T_1$ and $T_2$, if $T_2$ starts after $T_1$ commits:
$$\text{CommitTime}(T_1) < \text{StartTime}(T_2)$$
Guaranteed by holding the commit response until the current absolute time is guaranteed to have passed the chosen commit timestamp.
