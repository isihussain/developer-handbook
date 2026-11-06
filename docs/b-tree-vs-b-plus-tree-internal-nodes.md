# B-Tree vs B+ Tree Storage Engine Architecture

Relational and KV storage engines rely on B+ Trees for disk-backed indexing.

```mermaid
graph TD
    subgraph BTree [B-Tree Node Structure]
        B1["[ Key 10 | Data ptr ] [ Key 20 | Data ptr ]"]
        B1 --> B2["Child (<10)"]
        B1 --> B3["Child (10..20)"]
    end
    subgraph BPlusTree [B+ Tree Node Structure]
        BP1["Internal Node: [ Key 10 ] [ Key 20 ] (Guide Keys Only)"]
        BP1 --> BP2["Leaf: [10 | Ptr] [15 | Ptr]"]
        BP1 --> BP3["Leaf: [20 | Ptr] [25 | Ptr]"]
        BP2 <-->|Doubly Linked List| BP3
    end
```

---

## 1. Fanout Advantage

- Internal nodes hold keys and child pointers only, fitting $300	ext{--}500$ keys per 4KB page.
- Leaf nodes are linked in a bidirectional chain, making range queries $O(K)$ sequential memory scans.
