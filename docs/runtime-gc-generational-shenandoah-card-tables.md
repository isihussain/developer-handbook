# JVM Internals: Generational Shenandoah & Card Table Filtering

Generational Shenandoah splits the heap into Young and Old generations while preserving concurrent evacuation, using card tables to track cross-generational references without stop-the-world barriers.

```mermaid
flowchart LR
    OldObject[Old Generation Object] -->|Writes reference to Young Object| Mutator[Mutator Write Barrier]
    Mutator --> CardTable[Mark Byte in Card Table as DIRTY (0x01)]
    CardTable --> Scanner[Concurrent Marking: Scans only Dirty Cards]
    Scanner --> Evacuate[Evacuates Young Objects Concurrently]
```

---

## 1. Core Mechanics

* **Load-Reference Barrier (LRB):** Intercepts memory loads to ensure mutators read the updated copy of an object during concurrent evacuation phases.
* **Card Table Resolution:** Memory is partitioned into 512-byte cards. When an Old object mutates a reference to a Young object, the write barrier marks the corresponding card dirty:

$$	ext{CardAddress} = 	ext{CardTableBase} + (	ext{ObjectAddress} \gg 9)$$

* **Concurrent Remembered Set Scan:** During Young GC collection passes, the collector scans dirty cards rather than traversing the entire Old generation heap.
