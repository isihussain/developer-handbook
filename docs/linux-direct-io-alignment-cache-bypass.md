# High-Performance I/O: O_DIRECT Alignment Constraints

Opening files with `O_DIRECT` instructs the Linux kernel to bypass the Page Cache entirely, streaming data directly between user buffers and NVMe storage via DMA.

```mermaid
flowchart TD
    DirectIO[write() with O_DIRECT] --> CheckAlignment{Is Buffer Pointer, File Offset, & Size Aligned to 512B/4KB?}
    CheckAlignment -->|Yes| DMA[DMA Transfer Directly from Userspace RAM to NVMe]
    CheckAlignment -->|No| Error[Return -EINVAL (Invalid Argument)!]
```

---

## 1. When to Use
- Custom database buffer managers (PostgreSQL, InnoDB, ScyllaDB) that implement their own caching logic and want to avoid double-buffering in Linux Page Cache.
