# RocksDB Write Stall Mitigation & Compaction Tuning

LSM engines throttle active writers when background flush or compaction threads fall behind incoming ingestion rates.

---

## 1. Primary Throttle Triggers

- `level0_slowdown_writes_trigger` (default 20 files): Delays writes.
- `level0_stop_writes_trigger` (default 36 files): Completely blocks writing threads.
- Fix: Increase background compaction threads (`max_background_jobs = 8`) and expand write buffer count.
