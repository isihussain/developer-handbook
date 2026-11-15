# Epoch-Based Reclamation (EBR) Lock-Free Architecture

Epoch-Based Reclamation permits safe memory deallocation in lock-free concurrent data structures without hardware hazard pointers or garbage collection by tracking reader execution epochs.

```mermaid
flowchart TD
    GlobalEpoch[Global Epoch Counter: e mod 3] --> Thread1[Reader Thread 1: Active in Epoch 0]
    GlobalEpoch --> Thread2[Reader Thread 2: Active in Epoch 0]
    GlobalEpoch --> Writer[Writer Thread: Retires Node X in Epoch 0]
    Writer --> RetireList[Append Node X to Retire List of Epoch 0]
    Reclaim[Reclaimer Check: All threads advanced past Epoch 0?]
    RetireList --> Reclaim
    Reclaim -->|Yes: Safe to Free| FreeMemory[Deallocate Node X Memory]
```

---

## 1. 3-Epoch Rotation Guarantee

Memory retired in epoch $e$ can only be referenced by threads that entered the data structure in epoch $e$. Once all threads announce an active epoch of $e+1$ or higher, no thread holds references to epoch $e$ retire lists, making deallocation safe.
