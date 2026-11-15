# Dynamo-Style Consistent Hashing & Token Ring Architecture

Decentralized distributed storage engines distribute key ranges across an integer hash ring using virtual nodes, maintaining balance during node membership changes and node failures.

```mermaid
flowchart TD
    subgraph Hash Ring: 0 to 2^32 - 1
        N1[Node A - Vnode 1] --> N2[Node B - Vnode 1]
        N2 --> N3[Node C - Vnode 1]
        N3 --> N4[Node A - Vnode 2]
        N4 --> N1
    end
    Key[Key: user_id_9421] -->|MD5/Murmur3 Hash| Position[Ring Position]
    Position -->|Clockwise traversal| N2
```

---

## 1. Virtual Node Invariant

Assigning each physical server multiple (e.g. 128-256) virtual node positions around the ring evens out distribution variance and prevents hotspotting when hardware is added or decommissioned.
