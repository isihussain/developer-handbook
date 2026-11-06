# Linux Kernel SLUB Allocator Architecture

The SLUB allocator manages small kernel object allocations (`kmalloc`), eliminating fragmentation from page-level buddy allocation.

```mermaid
graph TD
    Core[CPU Core] --> CPUList[kmem_cache_cpu: Per-CPU freelist (Lockless)]
    CPUList -.->|Exhausted| Node[kmem_cache_node: Per-NUMA partial slabs (Spinlock)]
    Node -.->|Exhausted| Buddy[Page Allocator: alloc_pages()]
```

---

## 1. Allocation Tiers

1. **Fast Path (Per-CPU Freelist)**: Uses pointer exchange on local CPU struct without acquiring locks.
2. **Medium Path (Partial Slab)**: Pulls a free object from the per-NUMA node partial slab list under a spinlock.
3. **Slow Path (Buddy Allocator)**: Allocates contiguous physical memory pages via `alloc_pages()`.
