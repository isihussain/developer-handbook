# Database Reliability: Zero-Downtime Expand and Contract Pattern

Renaming or dropping database columns in production without service downtime requires phased multi-deployment synchronization.

```mermaid
flowchart TD
    Phase1["Phase 1 (Expand): Add new column 'full_name' alongside old 'name'"] --> Phase2["Phase 2: App dual-writes to both columns, reads from old"]
    Phase2 --> Phase3["Phase 3: Backfill historical rows from 'name' to 'full_name'"]
    Phase3 --> Phase4["Phase 4: App reads from 'full_name', dual-writes"]
    Phase4 --> Phase5["Phase 5 (Contract): Remove old column 'name' safely!"]
```

---

## 1. Golden Rule
Never rename a column in a single migration script while active web servers are running.
