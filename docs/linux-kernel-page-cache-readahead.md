# Linux Kernel: Page Cache Readahead & Window Scaling

The Linux kernel file system layer optimizes sequential I/O by managing dynamic readahead windows, pre-fetching blocks into page cache before user applications request them.

```mermaid
flowchart LR
    subgraph ReadSequence[Sequential Read Pattern]
        Current[Current Window: Pages Requested by Application]
        Readahead[Readahead Window: Asynchronously Dispatched to NVMe]
    end
    Current -->|Hit readahead page| Scale["Scale window up: max_sectors_kb (e.g. 512KB)"]
    Current -->|Non-sequential seek| Reset["Drop readahead window: fall back to single page read"]
```

---

## 1. Tuning & Invariants

* **`posix_fadvise()`:** Applications signal read patterns (`POSIX_FADV_SEQUENTIAL` doubles readahead window; `POSIX_FADV_RANDOM` disables readahead completely).
* **Asynchronous Flags:** The kernel flags the midpoint page with `PageReadahead`. Touching this page triggers the next background readahead burst without blocking the application.
