# File Systems: XFS Allocation Groups (AG) & Concurrency Architecture

XFS scales multi-threaded write performance on enterprise NVMe arrays by partitioning file systems into independent Allocation Groups (AG), each maintaining dedicated B+tree allocators.

```mermaid
flowchart LR
    subgraph XFS_Disk[XFS File System: 8 Allocation Groups]
        AG0[AG 0: Independent B+Trees]
        AG1[AG 1: Independent B+Trees]
        AG2[AG 2: Independent B+Trees]
        AG7[AG 7: Independent B+Trees]
    end
    subgraph AG_Internals[Allocation Group Data Structures]
        bnobt[bnobt: Free space indexed by block number]
        cntbt[cntbt: Free space indexed by extent size]
        inobt[inobt: Inode allocation B+tree]
    end
    AG0 --> AG_Internals
```

---

## 1. Concurrency & Allocation Mechanics

* **Parallel Allocation:** Threads allocating blocks in disjoint Allocation Groups acquire separate AG header locks, eliminating global allocator serialization.
* **Dual Free Space B+Trees:**
  - `bnobt`: Enables fast coalescing of neighboring free extents.
  - `cntbt`: Enables $O(\log N)$ lookups for best-fit contiguous extent allocations.
* **Delayed Allocation (Delalloc):** Buffers allocation decisions until writeback, optimizing contiguous block placement and mitigating file fragmentation.
