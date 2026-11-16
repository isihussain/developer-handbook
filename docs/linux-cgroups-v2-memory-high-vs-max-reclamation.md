# Linux Resource Isolation: cgroups v2 Memory Pressure Mechanics

cgroups v2 introduces a graduated, non-catastrophic memory pressure model through `memory.high` (soft throttle) versus `memory.max` (hard limit with OOM killer).

```mermaid
flowchart TD
    Usage[cgroup Memory Usage] --> CheckHigh{Usage > memory.high?}
    CheckHigh -->|No| Normal[Normal Execution]
    CheckHigh -->|Yes| Throttle[Proportional Delay Added to Allocating Threads]
    Throttle --> DirectReclaim[Direct Reclaim Background Pressure]
    DirectReclaim --> CheckMax{Usage > memory.max?}
    CheckMax -->|No| Recovered[Usage Drops Below memory.high]
    CheckMax -->|Yes| OOM[Invoke OOM Killer!]
```

---

## 1. Tuning Strategy
- Set `memory.high` at 80-85% of container threshold to trigger aggressive background page reclaim before triggering latency spikes or process termination.
