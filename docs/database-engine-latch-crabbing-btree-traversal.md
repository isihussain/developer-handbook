# Database Engines: B-Tree Latch Crabbing & Optimistic Coupling

Latch Crabbing (Lock Coupling) governs concurrent access to B-trees by acquiring child latches before releasing parent latches, ensuring safe tree modifications without global synchronization.

```mermaid
flowchart TD
    subgraph ReaderCrabbing[Reader Latch Crabbing: Top-Down]
        R_Root[Acquire Shared Read Latch: Root] --> R_Child[Acquire Shared Read Latch: Child]
        R_Child --> R_ReleaseParent[Release Shared Latch: Root]
    end
    subgraph WriterCrabbing[Writer Latch Crabbing: Top-Down]
        W_Root[Acquire Exclusive Write Latch: Root] --> W_Child[Acquire Exclusive Write Latch: Child]
        W_Child --> W_CheckSafe{Is Child Safe? Not Full / Not Underflow}
        W_CheckSafe -->|Safe: Cannot propagate split upward| W_ReleaseParent[Release All Ancestor Exclusive Latches]
        W_CheckSafe -->|Unsafe: Might split| W_HoldAncestors[Retain All Ancestor Latches]
    end
```

---

## 1. Latch Invariants

* **Safe Nodes:** A node is "safe" for an insertion if `keys.length < MAX_KEYS` (cannot trigger a split); safe for a deletion if `keys.length > MIN_KEYS`.
* **Optimistic Coupling:** Modern engines read nodes using version numbers without latches. If a version mismatch is detected after payload reading, the thread retries with explicit shared latches.
