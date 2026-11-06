# Lock-Free Memory Reclamation: Hazard Pointers

Hazard pointers (Maged Michael) prevent the Use-After-Free and ABA problems in lock-free data structures without requiring garbage collection.

```mermaid
sequenceDiagram
    autonumber
    Reader->>HP: Publish target pointer P to private Hazard Pointer slot
    Reader->>Reader: Read data safely from P
    Writer->>Writer: Unlink P from lock-free data structure
    Writer->>Writer: Add P to retired node list
    Note over Writer: Scan all published Hazard Pointers
    Writer->>Writer: Free nodes NOT protected by any Hazard Pointer
    Reader->>HP: Clear Hazard Pointer slot
```

---

## 1. Principles

- Reader threads publish memory addresses they are actively inspecting into per-thread hazard pointer slots.
- Reclaiming threads accumulate retired nodes and only call `free()` on pointers not appearing in any active hazard slot.
