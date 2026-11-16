# Distributed Storage: Gossip Protocol Anti-Entropy with Merkle Trees

To synchronize divergent data across nodes with minimal network bandwidth, systems like Cassandra and Dynamo build hierarchical Merkle trees over hash ranges.

```mermaid
flowchart TD
    Root[Root Hash: Mismatch!] --> Left[Left Child: Match]
    Root --> Right[Right Child: Mismatch!]
    Right --> Leaf1[Key Range 10-14: Match]
    Right --> Leaf2[Key Range 15-19: DIFFERENCE FOUND!]
    Leaf2 --> Stream[Only Replicate Range 15-19]
```

---

## 1. Algorithmic Complexity
- Tree comparison cost: $O(\log N)$ round-trips to isolate the exact corrupted range.
- Network payload: Proportional to the number of conflicting keys, not total dataset size.
