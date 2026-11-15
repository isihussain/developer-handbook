# Concurrency: Epoch-Based Memory Reclamation (EBR)

Epoch-Based Reclamation enables lock-free data structures to safely deallocate memory without risk of use-after-free bugs by tracking global and thread-local epoch counters.

```mermaid
flowchart LR
    subgraph EpochTracker[Global Epoch Counter: e in {0, 1, 2}]
        GlobalEpoch[Current Global Epoch = 1]
    end
    subgraph Threads[Per-Thread Active Epochs]
        T1[Thread 1: Active in Epoch 1]
        T2[Thread 2: Active in Epoch 1]
        T3[Thread 3: Inactive / Quiescent]
    end
    subgraph RetireQueue[Retire Limbo Lists]
        R0[Epoch 0: Safe to free! No active threads in Epoch 0]
        R1[Epoch 1: Pending Deallocation]
        R2[Epoch 2: Inactive]
    end
```

---

## 1. Three-Epoch Rule

* **State Tracking:** Threads enter critical sections by registering the current global epoch ($e_{	ext{global}}$) in their thread-local variable and clear it upon exit.
* **Epoch Advance:** The global epoch increments modulo 3 ($e \leftarrow (e + 1) mod 3$) once all active threads have exited epoch $e - 1$.
* **Safe Reclamation:** Any memory retired during epoch $e - 2 \pmod 3$ can be reclaimed immediately because no active thread holds a reference to it.
