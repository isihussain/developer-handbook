# Linux Virtual Memory: mmap() Flags and Copy-On-Write (COW) Semantics

The `mmap()` system call maps files directly into process address space with distinct sharing and persistence guarantees.

```mermaid
flowchart TD
    FileOnDisk[File on Disk: /data/db.bin] --> PageCache[Linux Page Cache]
    PageCache --> MmapChoice{mmap Mapping Flags}
    MmapChoice -->|MAP_SHARED| Shared["Writes modify Page Cache directly: Visible to all processes & flushed to disk"]
    MmapChoice -->|MAP_PRIVATE| Private["Copy-On-Write (COW): Writes allocate private anonymous page: Discarded on exit!"]
```

---

## 1. Fork Optimization
`fork()` maps all parent memory pages into the child as `MAP_PRIVATE` with read-only PTE flags. Only when either process writes to a page does a minor page fault allocate a physical copy.
