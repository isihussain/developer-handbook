# Linux Storage: Page Cache Dirty Ratio and Writeback Architecture

When applications invoke `write()`, pages in the Linux Page Cache become marked as dirty. Background kernel flusher threads periodically sync them to disk.

```mermaid
flowchart TD
    AppWrite[write() System Call] --> PageCache[Page Cache: Marked PG_dirty]
    PageCache --> CheckBackground{"Dirty % > vm.dirty_background_ratio?"}
    CheckBackground -->|Yes| Flusher[Wake up wb_workqueue flusher threads]
    CheckBackground -->|No| CheckMax{"Dirty % > vm.dirty_ratio?"}
    CheckMax -->|Yes| SynchronousStall[Block Application Thread until sync completes!]
```

---

## 1. Critical sysctl Tunables
- `vm.dirty_background_ratio`: Percentage of total memory when background writing starts (default 10%).
- `vm.dirty_ratio`: Percentage of total memory where writers are blocked (default 20%).
