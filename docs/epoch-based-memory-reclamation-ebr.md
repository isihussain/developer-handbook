# Concurrency Internals: Epoch-Based Memory Reclamation (EBR)

In lock-free data structures (crossbeam, jemalloc), threads cannot free nodes immediately because concurrent readers may still hold pointers. EBR tracks generational epochs.

```mermaid
flowchart TD
    GlobalEpoch[Global Epoch: e = 2] --> Thread1[Thread 1: Local Epoch = 2]
    GlobalEpoch --> Thread2[Thread 2: Local Epoch = 2]
    DeleteOp[Delete Node X] --> Retire[Retire Node X to Epoch 2 Trash List]
    subgraph Reclamation
        AllThreadsAdvance{All active threads reach Epoch >= 3?}
        AllThreadsAdvance -->|Yes| FreeMemory["kfree(Node X): Safe! Zero concurrent readers exist!"]
    end
```

---

## 1. Overhead
EBR introduces near-zero overhead during active reads compared to atomic reference counting or Hazard Pointers.
