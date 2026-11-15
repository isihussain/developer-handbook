# Concurrency: Mellor-Crummey & Scott (MCS) Lock Architecture

The MCS lock is a scalable queue lock where each contending thread spins strictly on its own thread-local cache line, eliminating bus contention and cache coherence traffic on multi-socket SMP systems.

```mermaid
flowchart LR
    Tail[Global Tail Pointer] --> Node3[Thread 3 MCS Node: locked = true]
    Node3 --> Node2[Thread 2 MCS Node: locked = true]
    Node2 --> Node1[Thread 1 MCS Node: locked = false (Owner)]
```

---

## 1. Operating Invariants

```c
struct mcs_node {
    struct mcs_node *next;
    bool locked;
};
```

1. **Lock Acquisition:**
   - Thread allocates local `mcs_node`.
   - Atomically swaps global tail pointer: `prev = atomic_exchange(&lock->tail, &my_node)`.
   - If `prev != NULL`, sets `prev->next = &my_node` and spins on local `my_node.locked`.
2. **Lock Release:**
   - Thread checks if `my_node.next == NULL`. If so, uses CAS to clear tail pointer.
   - Otherwise, sets `my_node.next->locked = false`, directly waking the next waiting thread.
