# Chandy-Lamport Distributed Snapshot Algorithm

Captures consistent global states in distributed systems without pausing active message processing or requiring synchronized clocks.

```mermaid
sequenceDiagram
    autonumber
    Note over P1: P1 initiates snapshot
    P1->P1: Record local state
    P1->>P2: Marker(M) on Channel c12
    Note over P1: Record messages arriving on c21
    P2->>P1: Normal Message (m1)
    Note over P2: Receives Marker(M) on c12
    P2->P2: First marker: Record local state
    P2->P2: Mark c12 state as EMPTY
    P2->>P1: Marker(M) on Channel c21
    Note over P1: Receives Marker(M) on c21
    P1->P1: Close recording for c21 (m1 captured in channel)
```

---

## 1. Assumptions

- FIFO, lossless, and directed communication channels between nodes.
- Strongly connected process graph.
