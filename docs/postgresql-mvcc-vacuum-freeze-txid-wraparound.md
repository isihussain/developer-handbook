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

---

## 2. What freezing changes

In PostgreSQL 9.4 and later, freezing normally sets a tuple flag; it does not overwrite the tuple's original xmin with FrozenTransactionId (2). This preserves the original XID for inspection while making the tuple visible to all transactions. Old databases upgraded from pre-9.4 versions may still contain tuples whose xmin is 2.

The wraparound limit is based on the age of the oldest unfrozen XID, not a timer that expires at a fixed wall-clock interval. Autovacuum's anti-wraparound work must complete before the relevant XIDs become unsafe.

## References

- PostgreSQL, [Routine vacuuming: preventing transaction ID wraparound](https://www.postgresql.org/docs/current/routine-vacuuming.html#VACUUM-FOR-WRAPAROUND).
