# Go Internals: Tricolor Mark-Sweep Garbage Collector

The Go runtime achieves sub-millisecond stop-the-world (STW) pauses using a concurrent, non-generational, non-moving tricolor mark-sweep collector.

```mermaid
flowchart TD
    White[White Set: Unvisited Objects (Eligible for GC)]
    Grey[Grey Set: Discovered Objects whose children are unvisited]
    Black[Black Set: Live Objects with all reachable children scanned]
    Grey -->|Scan Pointers| Black
    White -->|Reachable| Grey
    subgraph STW_Pacing
        WriteBarrier[Write Barrier: Enforces Dijkstra / Yuasa invariant during concurrent mutator execution]
    end
```

---

## 1. Write Barrier Invariant
Prevents the mutator (running Go goroutines) from hiding reachable white objects behind black objects without passing through grey tracking.
