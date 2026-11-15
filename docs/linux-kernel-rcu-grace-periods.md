# Linux Kernel Read-Copy-Update (RCU) Architecture

RCU guarantees deterministic, zero-synchronization read access to shared kernel data structures by delaying memory reclamation until all pre-existing read-side critical sections complete a Grace Period.

```mermaid
flowchart TD
    Reader1[Reader 1: rcu_read_lock] --> ReadOld[Access Original Data Structure]
    Writer[Writer Thread] --> Copy[Clone Original Node -> Mutate New Copy]
    Copy --> Publish[rcu_assign_pointer: Atomic pointer swap to New Copy]
    Publish --> Sync[synchronize_rcu: Await Grace Period / Quiescent States]
    ReadOld --> Unlock1[rcu_read_unlock / Context Switch: Quiescent State reached]
    Sync --> Free[kfree: Deallocate Original Node]
```

---

## 1. Quiescent States

A quiescent state occurs on a CPU core when it is guaranteed not to hold references to pre-existing RCU read-side critical sections:
* CPU context switch (in non-preemptible RCU).
* Transition to userspace execution.
* CPU entering idle loop (`nohz`).
