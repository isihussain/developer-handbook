# Linux cgroup v2 Memory Controller & OOM Scoring

cgroup v2 establishes a unified single-hierarchy resource control system under `/sys/fs/cgroup`.

```mermaid
graph TD
    Root[/sys/fs/cgroup] --> Svc[services.slice]
    Svc --> DB[database.scope]
    DB --> Limit["memory.max (Hard limit, triggers OOM)"]
    DB --> High["memory.high (Soft limit, throttles)"]
    DB --> Low["memory.low (Best-effort protection)"]
    DB --> Min["memory.min (Hard protection, never reclaimed)"]
```

---

## 1. Protection Thresholds

- `memory.min`: Guaranteed minimum RAM; kernel will never reclaim pages below this threshold.
- `memory.low`: Best-effort protection; reclaimed only if unassigned memory is exhausted.
- `memory.high`: Throttling boundary; processes are delayed and forced to reclaim their own pages.
- `memory.max`: Hard ceiling; exceeding triggers the kernel Out-Of-Memory (OOM) killer.
