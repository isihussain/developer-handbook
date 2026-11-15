# Concurrency: Hazard Pointers & Lock-Free Memory Safety

Hazard Pointers provide lock-free safe memory reclamation by allowing reader threads to publish pointers to active nodes, preventing writer threads from deallocating memory currently being read.

```mermaid
sequenceDiagram
    participant Reader as Reader Thread
    participant HP as Global Hazard Pointer Slot
    participant Node as Target Node A
    participant Writer as Writer / Deleting Thread
    Reader->>Node: Reads pointer to Node A
    Reader->>HP: Publishes HP[thread_id] = &Node A
    Reader->>Node: Re-validates that Node A is still reachable
    Writer->>Node: Unlinks Node A from lock-free list
    Note over Writer: Scans all global HP slots before free()
    Note over Writer: Detects HP matching &Node A -> Defers deallocation to retire list!
    Reader->>HP: Clears HP[thread_id] = NULL
```

---

## 1. Algorithm Invariants

1. **Publish Before Dereference:** A reader must write the target address into its hazard pointer slot and issue a memory fence (`atomic_thread_fence(memory_order_seq_cst)`) before reading node data.
2. **Re-Validation Check:** The reader must verify that the node was not unlinked by a writer immediately prior to the hazard pointer publication.
3. **Bounded Memory:** Unlike EBR, hazard pointers guarantee bounded memory consumption even if a single reader thread stalls indefinitely inside a critical section.
