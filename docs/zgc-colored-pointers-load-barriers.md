# ZGC Colored Pointers & JIT Load Barriers Architecture

The Z Garbage Collector achieves sub-millisecond maximum pause times by embedding heap metadata directly into pointer address bits and repairing stale object references concurrently via load barriers.

```mermaid
flowchart LR
    subgraph 64-bit Pointer Anatomy
        Unused[16 bits: Unused]
        Marked0[1 bit: Marked0]
        Marked1[1 bit: Marked1]
        Remapped[1 bit: Remapped]
        Address[44 bits: Physical Object Address 16TB max]
    end
```

---

## 1. Concurrent Load Barrier Protocol

When user code dereferences an object pointer (`obj.field`), a JIT-compiled load barrier tests the colored status bit:

* **Good Color:** Pointer is up to date; dereference proceeds without intervention (~2-3 CPU instructions).
* **Bad Color:** Object has moved during concurrent evacuation; slow path queries the forwarding table, repairs the reference, and updates the pointer in place.
