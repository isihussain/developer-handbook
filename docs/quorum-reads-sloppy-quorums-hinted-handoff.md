# High Availability: Sloppy Quorums and Hinted Handoff

When network partitions prevent strict quorum reads ($R + W > N$), Dynamo-style systems degrade gracefully into sloppy quorums with hinted handoffs.

```mermaid
sequenceDiagram
    participant Client
    participant NodeA as Node A (Primary - DOWN)
    participant NodeB as Node B (Primary)
    participant NodeC as Node C (Fallback Node)

    Client->>NodeB: Write Request
    NodeB-->>Client: Ack
    Client->>NodeC: Write with Hint: 'Deliver to Node A when online'
    NodeC-->>Client: Ack (Sloppy Quorum Satisfied!)
    Note over NodeA: Node A recovers
    NodeC->>NodeA: Hand off hinted mutations
```

---

## 1. Strict vs Sloppy Quorum
- **Strict Quorum:** $R + W > N$ over the fixed preference list. Guarantees read-your-writes consistency.
- **Sloppy Quorum:** Allows writes to healthy non-preference nodes during partition, prioritizing availability over consistency (AP in CAP).
