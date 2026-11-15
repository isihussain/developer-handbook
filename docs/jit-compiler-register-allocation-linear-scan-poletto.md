# JIT Compilation: Linear Scan Register Allocation (Poletto & Sarkar)

Linear Scan Register Allocation allocates physical CPU registers in $O(V)$ linear time over live variable intervals, providing JIT compilers with fast code generation compared to graph coloring.

```mermaid
flowchart LR
    LiveIntervals[Live Intervals: Sorted by start point] --> ActiveList[Active Interval List: Sorted by end point]
    ActiveList --> Expire[Expire Old Intervals: Frees registers whose ranges ended]
    ActiveList --> Allocate{Available Register?}
    Allocate -->|Yes| Assign[Assign Free Physical Register]
    Allocate -->|No: Register Pressure Exceeded| Spill[Spill Candidate with Longest Lifetime to Stack]
```

---

## 1. Algorithm Invariants

1. **Sorting:** Live intervals $[start, end]$ are computed via liveness analysis and sorted by ascending $start$ positions.
2. **Active List Maintenance:** Active intervals currently holding registers are maintained sorted by ascending $end$ points.
3. **Spill Heuristic:** When physical registers are exhausted, the allocator compares the current interval against active intervals, spilling the variable that extends farthest into the future (`SpillAtInterval`).
