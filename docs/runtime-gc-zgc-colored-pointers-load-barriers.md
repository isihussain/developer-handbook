# Runtime Systems: OpenJDK ZGC Colored Pointers & Load Barriers

The OpenJDK Z Garbage Collector (ZGC) achieves sub-millisecond maximum pause times across terabyte-scale heaps using 64-bit reference colored pointers and concurrent JIT load barriers.

```mermaid
flowchart TD
    subgraph PointerFormat[64-bit Virtual Reference Layout]
        AddressBits[Bits 0-43: Object Offset Address - 16TB Space]
        Finalizable[Bit 44: Finalizable Flag]
        Remapped[Bit 45: Remapped Reference Flag]
        Marked1[Bit 46: Marked1 Live Object Flag]
        Marked0[Bit 47: Marked0 Live Object Flag]
    end
    AppThread[Application Mutator Thread: Dereferences Field] --> LoadBarrier{Is Pointer Colored Remapped?}
    LoadBarrier -->|Yes: Fast Path| ObjectAccess[Direct Field Access in Cache: ~1 cycle]
    LoadBarrier -->|No: Slow Path| Forward[Looks up Forwarding Table: Self-Heals Reference]
    Forward --> ObjectAccess
```

---

## 1. Operating Mechanics

* **Colored Pointers:** Metadata bits reside inside reference pointer words rather than object headers, allowing ZGC to inspect object liveness without dereferencing heap memory.
* **Self-Healing Load Barrier:** When a thread loads a reference pointing to an object relocated during concurrent evacuation, the load barrier intercepts the read, updates the pointer in place to the new forwarding address, and completes the read without blocking other threads.
* **Address Space Aliasing:** Uses page table aliasing on Linux (`memfd_create`) to map `Marked0`, `Marked1`, and `Remapped` address spaces to identical physical memory frames.
