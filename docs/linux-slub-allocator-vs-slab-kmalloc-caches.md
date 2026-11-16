# Kernel Internals: SLUB Allocator vs Legacy SLAB

The SLUB allocator is the default Linux kernel slab allocator, eliminating complex per-CPU object queues and queues-of-queues found in legacy SLAB.

```mermaid
flowchart TD
    Kmalloc[kmalloc request: e.g. 128 bytes] --> KM_Cache[kmalloc-128 Cache]
    KM_Cache --> CpuSlab[Per-CPU Slab: Lockless Fast Path]
    CpuSlab -->|Empty| NodeList[Per-Node Partial List: Locked Slow Path]
    NodeList -->|Empty| Buddy[Buddy Allocator: Allocates new page frame]
```

---

## 1. Fast Path vs Slow Path
- **Fast Path:** Uses atomic cmpxchg on per-CPU slab pointer without taking any spinlocks.
- **Slow Path:** Acquires the per-node partial spinlock to obtain a new page of objects.
