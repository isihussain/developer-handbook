# WiredTiger B-Tree Hazard Pointers & Lock-Free Traversal

WiredTiger (MongoDB's storage engine) achieves high concurrent read throughput by protecting in-memory B-Tree nodes with hazard pointers, preventing memory reclamation during lock-free traversal.

```mermaid
sequenceDiagram
    participant Reader as Reader Thread
    participant HP as Global Hazard Pointer Array
    participant Node as B-Tree Node
    participant GC as Hazard Pointer Evictor

    Reader->>HP: Publish target Node pointer into Hazard Slot
    Reader->>Node: Read in-memory key/value payload
    Note over GC: Evictor attempts node free
    GC->>HP: Scan active Hazard Pointers
    Note over GC: Match found! Eviction postponed safely
    Reader->>HP: Clear Hazard Slot
```

---

## 1. Safe Memory Reclamation Guarantee

A retired B-Tree node is only freed or unmapped when a scan of all thread hazard slots confirms zero active references to the pointer.
