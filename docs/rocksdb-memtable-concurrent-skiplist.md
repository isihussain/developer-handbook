# RocksDB Concurrent SkipList MemTable Architecture

RocksDB buffers incoming writes in a lock-free probabilistic SkipList MemTable, offering $O(\log n)$ concurrent search and insert operations without coarse read/write locks.

```mermaid
flowchart LR
    subgraph Level 2 [Level 2 Express Lane]
        L2_1[Key 10] -------> L2_5[Key 50] -------> L2_9[Key 90]
    end
    subgraph Level 1 [Level 1 Intermediate Lane]
        L1_1[Key 10] -> L1_3[Key 30] -> L1_5[Key 50] -> L1_7[Key 70] -> L1_9[Key 90]
    end
    subgraph Level 0 [Level 0 Base Lane: All Keys]
        L0_1[Key 10] -> L0_2[Key 20] -> L0_3[Key 30] -> L0_4[Key 40] -> L0_5[Key 50] -> L0_6[Key 60] -> L0_7[Key 70] -> L0_8[Key 80] -> L0_9[Key 90]
    end
```

---

## 1. Concurrent Invariant

* **Inserts:** Append new nodes bottom-up using atomic pointer updates with `release` semantics.
* **Reads:** Traverse top-down using atomic `acquire` memory loads, completely free of locks.
