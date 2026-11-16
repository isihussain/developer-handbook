# High-Performance Raft: Pipelined AppendEntries and Batching

Vanilla Raft waits for an AppendEntries response before sending subsequent entries. Pipelining decouples throughput from round-trip network latency.

```mermaid
sequenceDiagram
    participant Leader
    participant Follower

    Leader->>Follower: AppendEntries (Index 101-105)
    Leader->>Follower: AppendEntries (Index 106-110 - In Flight Immediately!)
    Leader->>Follower: AppendEntries (Index 111-115 - In Flight Immediately!)
    Follower-->>Leader: Ack 105
    Follower-->>Leader: Ack 110
    Follower-->>Leader: Ack 115
```

---

## 1. In-Flight Window Control
- Leaders track maximum in-flight bytes or message counts.
- If a follower drops packets or rejects an append due to log divergence, the pipeline resets back to the follower's confirmed `matchIndex`.
