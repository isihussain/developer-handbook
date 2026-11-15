# PostgreSQL MVCC: Tuple Visibility & Vacuum Freeze Architecture

PostgreSQL stores multiple versions of modified rows directly in heap pages. Transaction IDs (`xmin`, `xmax`) determine visibility, requiring vacuum freezes to avert 32-bit transaction wraparound corruption.

```mermaid
flowchart TD
    Tuple[Heap Tuple Header: 23 bytes] --> XMIN[xmin: Creating Transaction ID]
    Tuple --> XMAX[xmax: Deleting/Updating Transaction ID]
    Tuple --> Infomask[t_infomask: HEAP_XMIN_COMMITTED / HEAP_XMAX_INVALID]
    Vacuum[Autovacuum Daemon] -->|Scans pages older than autovacuum_freeze_max_age| Freeze[Set HEAP_XMIN_FROZEN bit]
```

---

## 1. Visibility Rules

* If `xmin` is uncommitted, tuple is invisible to other transactions.
* If `xmin` is committed and `xmax = 0` (or aborted), tuple is visible.
* If `HEAP_XMIN_FROZEN` is set, tuple is unconditionally visible to all past, present, and future transactions.
