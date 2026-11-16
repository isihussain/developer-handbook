# High-Performance RPC: Cap'n Proto Arena Allocation Architecture

Cap'n Proto (by Kenton Varda) features infinite-speed serialization (memory-identical layout) and Promise Pipelining (Time-Travel RPC).

```mermaid
sequenceDiagram
    participant Client
    participant Server

    Note over Client: Call 1: user = getUser(id)
    Note over Client: Call 2: user.getAvatar() (Dispatched in SAME network flight!)
    Client->>Server: Pipeline Call 1 + Call 2 together!
    Note over Server: Server evaluates Call 2 on Call 1 promise before returning!
    Server-->>Client: Returns Avatar directly (1 RTT total!)
```

---

## 1. Promise Pipelining
Eliminates round-trip delays between dependent RPC calls: client pipelines call 2 referencing the unresolved promise of call 1.
