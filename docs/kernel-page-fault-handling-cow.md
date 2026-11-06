# Linux Kernel Page Fault Handling & Copy-on-Write (COW)

The Linux virtual memory management subsystem handles hardware MMU exceptions when an address translation is missing or violates permission bits.

```mermaid
flowchart TD
    CPU[CPU Instruction Read/Write] --> MMU[Hardware MMU Walk]
    MMU -->|Translation missing or write to RO page| Fault[Page Fault Exception: CR2 / FAR_EL1]
    Fault --> Handler[Kernel do_page_fault()]
    Handler --> Valid{Address in VMA?}
    Valid -->|No| Segfault[SIGSEGV: Invalid Memory Reference]
    Valid -->|Yes| Perm{Write on COW page?}
    Perm -->|Yes| COW[Copy page to new physical frame & remap RW]
    Perm -->|No| Demand[Demand paging: read from disk/swap]
    COW --> Resume[Resume user-space instruction]
    Demand --> Resume
```

---

## 1. Major vs Minor Page Faults

| Type | Cause | Disk I/O Required? | Latency Impact |
|---|---|---|---|
| **Minor Fault** | Page exists in physical memory (page cache, shared memory, or unmapped zero page). | No | Sub-microsecond (page table entry update only). |
| **Major Fault** | Page must be retrieved from durable block storage or swap. | Yes | Milliseconds (storage controller queue and transfer). |

---

## 2. Copy-on-Write (COW) Fork Mechanics

When a process invokes `fork()`:
1. Kernel duplicates Page Table Entries (PTEs) of the parent for the child.
2. Clears the write permission bit (`_PAGE_RW`) on all writable data pages in both parent and child.
3. Increments page frame reference count in `struct page`.
4. When either process attempts a write, MMU generates a permission page fault:
   - Kernel detects COW flag on the VMA.
   - If reference count is 1, marks page writable in-place.
   - If reference count $>1$, allocates a new page frame, copies contents (`memcpy`), decrements original reference count, and marks new frame writable.
