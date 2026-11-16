# Kubernetes Scheduling: Topology Spread Constraints Architecture

Topology Spread Constraints ensure pods are evenly distributed across failure domains (zones, regions, nodes) to prevent localized outages.

```mermaid
flowchart TD
    IncomingPod[New Pod to Schedule] --> CheckZone{Inspect Pod Count across Zone A, B, C}
    CheckZone --> Calc["Compute Skew = max(ZoneCount) - min(ZoneCount)"]
    Calc --> Rule{"Is Skew <= maxSkew (e.g. 1)?"}
    Rule -->|Yes| Schedule[Schedule Pod in least-populated zone]
    Rule -->|No| DoNotSchedule[DoNotSchedule / ScheduleAnyway based on whenUnsatisfiable]
```

---

## 1. Invariant
Enforces high availability without hard anti-affinity anti-patterns that prevent autoscaling.
