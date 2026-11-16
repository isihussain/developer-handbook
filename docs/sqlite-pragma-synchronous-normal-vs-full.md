# SQLite Performance: PRAGMA synchronous Tradeoffs

The `PRAGMA synchronous` directive controls how aggressively SQLite synchronizes database pages to physical disk via `fsync()`.

```mermaid
flowchart TD
    SyncSetting[PRAGMA synchronous] --> Choice{Setting Mode}
    Choice -->|FULL (2)| Slow["fsync() after every critical write: 100% crash durable, but capped at ~100 writes/sec on HDD"]
    Choice -->|NORMAL (1)| Fast["Syncs only during WAL checkpointing: 10x-50x faster! Completely durable against app crashes!"]
    Choice -->|OFF (0)| Dangerous["Zero fsync(): Risk of DB corruption on OS power loss"]
```

---

## 1. WAL Mode Synergy
In WAL mode, `PRAGMA synchronous = NORMAL` is completely safe against application crashes; data loss is only possible on sudden host operating system power failure.
