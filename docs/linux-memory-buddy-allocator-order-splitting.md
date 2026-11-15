# Linux Kernel: Buddy Allocator & Order Block Coalescing

The Linux kernel Buddy Allocator manages contiguous physical memory frames in power-of-two page orders (Order 0 = 4KB to Order 10 = 4MB) using fast bitwise buddy calculations.

```mermaid
flowchart TD
    Order3[Order 3: 32KB Page Block] --> Split{Split Block}
    Split --> B0[Buddy 0: 16KB (Order 2)]
    Split --> B1[Buddy 1: 16KB (Order 2)]
    B0 --> Allocated[Allocated to Kernel Driver]
    B1 --> FreeList[Placed on Order 2 Free List]
    Allocated -->|Freed| Merge{Is Buddy 1 Free?}
    FreeList --> Merge
    Merge -->|Coalesce| Order3_Restored[Order 3 Block Restored]
```

---

## 1. Bitwise Buddy Mathematics

Given a page frame number (PFN) at order $k$, the buddy frame's PFN is calculated via a single XOR operation:

$$	ext{buddy\_pfn} = 	ext{pfn} \oplus (1 \ll k)$$

* **Coalescing Rule:** When a block is freed, the allocator inspects `buddy_pfn`. If the buddy is free and shares identical order $k$, both blocks merge into order $k+1$.
* **Anti-Fragmentation:** Memory zones partition pages into `MIGRATE_UNMOVABLE`, `MIGRATE_RECLAIMABLE`, and `MIGRATE_MOVABLE` to protect high-order allocations from unmovable pinning.
