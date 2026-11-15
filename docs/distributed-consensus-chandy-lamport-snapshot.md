# Distributed Systems: Chandy-Lamport Global Snapshot Algorithm

The Chandy-Lamport algorithm captures a globally consistent state snapshot of a distributed system (node states and in-flight channel messages) without halting ongoing transaction processing.

```mermaid
sequenceDiagram
    participant P as Process P (Initiator)
    participant Q as Process Q
    Note over P: 1. Records local state<br/>2. Sends Marker M across channel c_pq
    P->>Q: Marker M (En route in channel)
    Note over Q: 3. Receives Marker M on c_pq:<br/>- If Q has not recorded state -> Records local state and marks c_pq as empty<br/>- Sends Marker M out on all outgoing channels
    Q->>P: Messages in flight before Marker arrive and are recorded as channel state
    Q->>P: Marker M
    Note over P: 4. Receives Marker on c_qp: Stops recording channel c_qp
```

---

## 1. Algorithm Invariants

1. **FIFO Channels:** Assumes all inter-process communication channels preserve FIFO message delivery.
2. **Strong Consistency:** Guarantees that the captured global snapshot represents a state that could have occurred in an actual sequential execution trace.
3. **Non-Blocking Execution:** Nodes continue executing normal application computation and receiving new requests while snapshot recording takes place.
