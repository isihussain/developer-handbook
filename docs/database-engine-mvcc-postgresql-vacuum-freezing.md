# Database Engines: PostgreSQL MVCC & Transaction ID (XID) Freezing

PostgreSQL implements Multi-Version Concurrency Control (MVCC) by appending new tuple versions into table pages, requiring `VACUUM` processes to reclaim dead rows and freeze 32-bit transaction IDs.

```mermaid
flowchart TD
    subgraph TupleHeader[Heap Tuple Header: struct HeapTupleHeaderData]
        xmin[xmin: Inserting Transaction ID]
        xmax[xmax: Deleting / Updating Transaction ID]
        infomask[t_infomask: HEAP_XMIN_COMMITTED \| HEAP_XMIN_FROZEN]
    end
    subgraph VacuumFreeze[Autovacuum Freezing Process]
        OldTuple[Old Tuple with xmin = 50,000,000] -->|Age exceeds vacuum_freeze_min_age| FreezeAction[Set HEAP_XMIN_FROZEN bitmask in infomask]
        FreezeAction --> FrozenTuple[Frozen Tuple: Considered committed for all past and future XIDs]
    end
```

---

## 1. 32-Bit XID Wraparound

* **XID Modulo Space:** Transaction IDs cycle through a circular 32-bit integer space ($2^{32} pprox 4.29 	ext{ billion}$ transactions). Half the space ($2^{31}$) lies in the past; half in the future.
* **Wraparound Hazard:** If an old tuple's $xmin$ is not frozen before the current XID advances past $xmin + 2^{31}$, the tuple appears to have been inserted in the future and becomes invisible (data loss).
* **Aggressive Autovacuum:** When a table reaches `autovacuum_freeze_max_age` (default 200M transactions), PostgreSQL triggers a forced emergency vacuum to freeze old rows.
