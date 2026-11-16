# Distributed Consensus: The Redlock Algorithm and Martin Kleppmann's Critique

Redlock attempts distributed mutual exclusion using multiple independent Redis nodes without consensus protocols, but fails under asynchronous network pauses.

```mermaid
sequenceDiagram
    participant Client1 as Client 1 (Acquires Lock)
    participant Redis as Redis Nodes (Quorum Lock Acquired)
    participant Client2 as Client 2
    participant Storage as Shared Storage (Database)

    Client1->>Redis: Acquire Lock (TTL = 10s)
    Note over Client1: Long GC Pause (Full GC for 15 seconds!)
    Note over Redis: Lock TTL expires!
    Client2->>Redis: Acquire Lock (SUCCESS!)
    Client2->>Storage: Writes data to shared resource
    Note over Client1: Resumes from GC pause! Still believes it holds lock!
    Client1->>Storage: Writes corrupted data over Client 2!
```

---

## 1. Proper Solution: Fencing Tokens
Every lock acquisition must return a monotonically increasing fencing token. The storage engine rejects any write carrying a fencing token lower than the highest token observed.
