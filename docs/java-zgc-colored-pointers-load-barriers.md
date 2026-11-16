# Java Runtime: ZGC Colored Pointers and Load Barriers

ZGC delivers predictable $<1\text{ms}$ max pause times regardless of heap size (from 16MB to 16TB) using 64-bit colored pointers and concurrent compaction.

```mermaid
flowchart LR
    subgraph Pointer_64bit[64-Bit ZGC Colored Virtual Address]
        Unused[16 bits Unused]
        Finalizable[1 bit Finalizable]
        Remapped[1 bit Remapped]
        Marked1[1 bit Marked1]
        Marked0[1 bit Marked0]
        Address[44 bits Object Offset: Up to 16TB!]
    end
    Pointer_64bit --> LoadBarrier[Hardware JIT Load Barrier: Self-heals pointer on dereference if object moved!]
```

---

## 1. Load Barrier Self-Healing
If a thread dereferences an object that was relocated during concurrent compaction, the load barrier intercepts the load, reads the relocation table, updates the pointer in-place, and returns the live object.
