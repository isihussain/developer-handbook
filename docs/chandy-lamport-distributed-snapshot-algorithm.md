# Distributed Systems: Chandy-Lamport Distributed Snapshot

The Chandy-Lamport algorithm captures a consistent global state (state of processes and in-flight channel messages) without pausing distributed execution.

```mermaid
sequenceDiagram
    participant P1 as Process 1 (Initiator)
    participant C as FIFO Communication Channel
    participant P2 as Process 2

    Note over P1: Record Local State P1
    P1->>C: Send Marker Message
    P1->>C: Regular Message M1
    Note over C: In-flight messages buffered
    C->>P2: Deliver Marker Message
    Note over P2: Record Local State P2 upon first marker
    Note over P2: Record channel state until marker received from all incoming channels
```

---

## 1. Fundamental Assumptions
- Channels are FIFO, reliable, and unidirectional.
- Processes do not crash during snapshot collection.
