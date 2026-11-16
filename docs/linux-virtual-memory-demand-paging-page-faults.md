# Linux Memory: Demand Paging and Page Fault Mechanics

Linux utilizes lazy allocation (demand paging): `malloc()` only reserves virtual address space (VMA); physical RAM is mapped on first touch via page fault.

```mermaid
flowchart TD
    Access[Process Accesses Virtual Address] --> MMU[Hardware MMU: Checks Page Table Entry]
    MMU -->|PTE Present = 0| PageFault[Trigger Page Fault Exception (Vector 14)]
    PageFault --> Type{Is Page Backed by File or Anonymous?}
    Type -->|Anonymous (Zeroed)| Minor[Minor Fault: Allocate zeroed page frame in RAM]
    Type -->|File-backed on Disk| Major[Major Fault: Block thread and issue Disk I/O read!]
```

---

## 1. Observability
- High **minor faults**: Normal memory allocation churn.
- High **major faults**: Severe disk I/O bottleneck; processes blocking on swap or disk page loading.
