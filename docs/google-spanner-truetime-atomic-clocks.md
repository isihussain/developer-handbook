# Google Spanner TrueTime & Commit-Wait Protocol Architecture

Google Spanner achieves global external consistency (serializability across geo-distributed datacenters) by bounding physical time uncertainty using GPS receivers and atomic clocks.

```mermaid
flowchart TD
    TrueTime[TrueTime API: TT.now returns earliest, latest with uncertainty epsilon] --> Bounds[Interval: t_earliest <= absolute_time <= t_latest]
    Bounds --> CommitWait[Commit-Wait Rule: Wait duration 2 * epsilon before releasing commit locks]
    CommitWait --> Consistency[Guarantees: If T2 starts after T1 commits, timestamp T2 > timestamp T1]
```

---

## 1. Commit-Wait Invariant

1. Coordinator picks commit timestamp $s = 	ext{TT.now().latest}$.
2. Coordinator delays releasing locks until $	ext{TT.now().earliest} > s$.
3. Guarantees no subsequent transaction across the globe can be assigned a timestamp $\le s$.
