# Linux: Pressure Stall Information (PSI) Metrics

Pressure Stall Information (PSI) quantifies resource saturation across CPU, Memory, and I/O by tracking the exact duration tasks spend waiting for hardware access.

```mermaid
flowchart LR
    Tasks[Runnable Tasks Queue] --> Metric[PSI Kernel Aggregator]
    Metric --> Some["some: At least one task is stalled waiting on resource"]
    Metric --> Full["full: 100% of non-idle tasks are stalled (Productive CPU is 0)"]
    Some & Full --> Triggers["epoll() notification on /proc/pressure/ threshold breach"]
```

---

## 1. Metrics Breakdown

* **`some` Metric:** Percentage of time during which at least one active task was blocked on resource starvation.
* **`full` Metric:** Percentage of time during which all runnable tasks were stalled simultaneously (complete system throughput loss).
* **Early OOM Handling:** Production Kubernetes clusters monitor `/proc/pressure/memory` to trigger proactive container evictions before kernel hard OOM kills occur.
