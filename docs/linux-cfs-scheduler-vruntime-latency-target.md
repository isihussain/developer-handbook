# Linux Kernel: CFS Scheduler vruntime and Latency Targets

The Completely Fair Scheduler (CFS) models an ideal multi-tasking CPU on real hardware using a red-black tree keyed by `vruntime` (virtual runtime).

```mermaid
flowchart TD
    RBTree[Red-Black Tree: Ordered by vruntime] --> Smallest[Leftmost Node: Min vruntime]
    Smallest --> Pick[Pick Next Task to Run on CPU Core]
    Pick --> Run[Task Executes for Time Slice: delta_exec]
    Run --> Update["vruntime += delta_exec * (NICE_0_LOAD / weight)"]
    Update --> Reinsert[Reinsert into RB-Tree]
```

---

## 1. Nice Values and Weight
A higher nice value (lower priority) gives lower weight, meaning `vruntime` increases faster, causing the task to get less total CPU time.
