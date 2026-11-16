# Linux Filesystem: Page Cache Adaptive Readahead Architecture

The Linux kernel monitors file access patterns: detecting sequential reading triggers asynchronous readahead to fetch subsequent disk blocks into RAM before the application asks.

```mermaid
sequenceDiagram
    participant App as Application Thread
    participant VFS as VFS Readahead Window
    participant Disk as NVMe Storage

    App->>VFS: read() Offset 0-64KB
    Note over VFS: Detects Sequential Stream!
    VFS->>Disk: Asynchronously read ahead next 512KB into Page Cache
    App->>VFS: read() Offset 64KB-128KB
    Note over VFS: Cache HIT! Immediate response from RAM without blocking on Disk!
```

---

## 1. Application Hinting
Applications can override kernel heuristics with `posix_fadvise(fd, offset, len, POSIX_FADV_SEQUENTIAL)` to double the readahead window.
