# Merkle Mountain Ranges (MMR) & Accumulators

A Merkle Mountain Range is an append-only cryptographic accumulator constructed as a list of balanced binary Merkle trees of decreasing power-of-two sizes, supporting $O(\log N)$ inclusion proofs and $O(1)$ append operations.

```mermaid
flowchart TD
    subgraph MMR["MMR with 11 Leaves (Peaks at sizes 8, 2, 1)"]
        Peak1[Peak 1: Height 3 - Covers 8 leaves]
        Peak2[Peak 2: Height 1 - Covers 2 leaves]
        Peak3[Peak 3: Height 0 - Covers 1 leaf]
        Peak1 --> Bagging[Bagging the Peaks: Hash Peak1, Peak2, Peak3]
        Peak2 --> Bagging
        Peak3 --> Bagging
        Bagging --> MMR_Root[MMR Root Hash]
    end
```

---

## 1. MMR vs Standard Merkle Trees

| Metric | Standard Balanced Merkle Tree | Merkle Mountain Range (MMR) |
|---|---|---|
| **Append Cost** | $O(N)$ or requires fixed tree depth. | $O(\log N)$ worst-case, $O(1)$ amortized. |
| **Historical Invalidation** | Appending alters root, rehashes branches. | **Append-only invariant:** Prior nodes never change index or hash. |
| **Pruning Support** | Complex sub-tree truncation. | Trivial leaf deletion by storing only peak hashes. |
| **Index Representation** | 1-based or 0-based leaf indexes. | Flat array post-order traversal indexing. |
