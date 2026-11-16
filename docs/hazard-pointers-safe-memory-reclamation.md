# Concurrency Internals: Hazard Pointers Architecture

Hazard Pointers provide lock-free memory reclamation by allowing reader threads to publish a list of pointers currently being accessed.

```mermaid
sequenceDiagram
    participant Reader
    participant HP as Global Hazard Pointer Array
    participant Writer as Writer Thread (Deleter)

    Reader->>HP: Publish: HP[thread_id] = node_ptr
    Note over Reader: Dereferences and reads node_ptr safely
    Note over Writer: Unlinks node_ptr from data structure
    Writer->>HP: Scans Global Hazard Pointer Array
    Note over Writer: node_ptr is present in HP[thread_id]!
    Note over Writer: Defers deallocation to retired list!
    Note over Reader: Finished read: HP[thread_id] = NULL
    Note over Writer: Next reclaim pass: node_ptr freed safely!
```

---

## 1. Bound Guarantee
The number of retired, unreclaimed nodes per thread is bounded by $O(H \times P)$ where $H$ is hazard pointers per thread and $P$ is total threads.
