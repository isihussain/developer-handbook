# PostgreSQL Internals: MVCC, VACUUM, and Transaction ID Wraparound

PostgreSQL stores multiple row versions (tuples) on table pages with `xmin` and `xmax` transaction identifiers, requiring regular VACUUM to reclaim dead space and prevent 32-bit transaction wraparound.

```mermaid
flowchart TD
    RowUpdate[UPDATE statement] --> DeadTuple[Old Row: xmax = current_tx]
    RowUpdate --> LiveTuple[New Row: xmin = current_tx, xmax = 0]
    DeadTuple --> AutoVacuum{Autovacuum Daemon}
    AutoVacuum --> Reclaim[Reclaims dead tuple space for future inserts]
    AutoVacuum --> Freeze[Freeze Old xmin values to FrozenXID (2)]
```

---

## 1. TXID Wraparound Crisis
- 32-bit transaction IDs wrap around every ~2.14 billion transactions ($2^{31}$).
- Without freezing, active transactions could interpret ancient historical rows as occurring in the future, rendering data invisible.
