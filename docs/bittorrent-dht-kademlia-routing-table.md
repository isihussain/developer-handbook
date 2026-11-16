# Peer-to-Peer Systems: Kademlia DHT XOR Metric Architecture

The BitTorrent Distributed Hash Table (DHT) organizes nodes across a 160-bit keyspace using the XOR distance metric: $d(x, y) = x \oplus y$.

```mermaid
flowchart TD
    TargetID[Target InfoHash 160-bit] --> Lookup[Find 8 Closest Nodes in k-bucket]
    Lookup --> Query[Iterative find_node RPCs: Alpha = 3 concurrent requests]
    Query --> Converge[Logarithmic Routing Convergence in O(log N) hops!]
```

---

## 1. Unidirectional Metric
XOR distance satisfies all metric space invariants (identity of indiscernibles, symmetry, triangle inequality).
