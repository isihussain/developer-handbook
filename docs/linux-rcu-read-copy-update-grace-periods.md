# Synchronization Primitives: Linux Read-Copy Update (RCU)

RCU delivers lockless, zero-overhead reads for shared pointer structures by allowing readers to run concurrently with writers.

```mermaid
sequenceDiagram
    participant Reader as Reader Thread
    participant Writer as Writer Thread
    participant Reclamation as RCU Garbage Collection

    Note over Reader: rcu_read_lock(); (Zero CPU cost - simply disables preemption)
    Reader->>Reader: Dereferences and reads current data struct
    Note over Writer: Allocates copy, mutates copy, updates pointer atomically!
    Note over Reader: rcu_read_unlock();
    Note over Writer: synchronize_rcu(); (Wait for Grace Period: All existing readers to finish)
    Writer->>Reclamation: kfree(old_data_struct);
```

---

## 1. Read-Side Cost
In non-preemptible kernels, `rcu_read_lock()` compiles to literally zero assembly instructions!
