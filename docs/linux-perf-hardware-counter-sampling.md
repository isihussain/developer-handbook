# Linux perf & Hardware PMU Counter Profiling

The `perf` subsystem configures CPU Performance Monitoring Units (PMUs) to sample microarchitectural performance events.

---

## 1. Critical Events

| Event | Metric | Optimization Insight |
|---|---|---|
| `cycles` | CPU clock cycles | Identifies hot compute loops. |
| `instructions` | Instructions retired | IPC calculation (`instructions / cycles`). |
| `cache-misses` | LLC cache misses | Evaluates memory access locality. |
| `branch-misses` | Mispredicted branches | Highlights unpredictable branching logic. |
