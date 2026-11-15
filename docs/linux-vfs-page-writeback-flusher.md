# Linux VFS: Dirty Page Writeback & Flusher Subsystems

The Linux Virtual File System (VFS) buffers file modifications in RAM and delegates persistent NVMe/SSD syncing to background `bdi-flusher` kernel threads (`wb_workfn`).

```mermaid
flowchart TD
    UserWrite[Application write syscall] --> PageCache[VFS Page Cache: Marked Dirty]
    PageCache --> CheckLimits{Dirty Page Ratio Threshold Check}
    CheckLimits -->|dirty_background_ratio reached (e.g. 10%)| AsyncFlush[Wake up bdi_writeback thread to flush in background]
    CheckLimits -->|dirty_ratio reached (e.g. 20%)| SyncThrottle[Throttle user thread: Block process until pages flushed]
```

---

## 1. Key Kernel Parameters

* **`vm.dirty_background_ratio`:** Percentage of total system memory at which background `kworker` threads begin writing dirty pages to disk asynchronously.
* **`vm.dirty_ratio`:** Hard upper threshold at which writing applications are blocked and forced into synchronous writeback mode.
* **`vm.dirty_expire_centisecs`:** Maximum age (in hundredths of a second) a page can remain dirty before being forced to disk.
