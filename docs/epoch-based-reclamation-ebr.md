# Epoch-Based Reclamation (EBR) Architecture

EBR provides low-overhead memory reclamation for lock-free and read-copy-update (RCU) algorithms.

```mermaid
flowchart TD
    E0[Global Epoch: e] --> T1[Thread 1: Enters epoch e]
    E0 --> T2[Thread 2: Enters epoch e]
    T1 --> Retire[Retire Node -> Placed in epoch e limbo bag]
    T1 --> Advance{All active threads in epoch e?}
    Advance -->|Yes| E1[Advance Global Epoch to e+1]
    E1 --> Free[Reclaim & free limbo bag from epoch e-1]
```

---

## 1. Epoch Cycles

- System maintains a global epoch counter ($e \in \{0, 1, 2\}$).
- Threads register upon entering a critical read section.
- Memory retired in epoch $e$ is guaranteed safe to delete once all threads have progressed past epoch $e$.
