# Google Spanner: TrueTime & External Consistency Architecture

Google Spanner achieves global linearizable transactions (external consistency) without coordination locks by bounding physical clock drift using GPS and atomic clocks.

```mermaid
flowchart TD
    TrueTime["TrueTime.now: Returns [earliest, latest] with uncertainty epsilon (<= 7ms)"]
    TrueTime --> CommitWait["Commit Wait Rule: Transaction commit timestamp s = latest"]
    CommitWait --> Wait["Wait until real time exceeds s (Absolute sleep >= 2*epsilon)"]
    Wait --> Visible["Transaction Guaranteed Visible Globally After Prior Commits"]
```

---

## 1. Key Invariants

1. **Clock Uncertainty ($\epsilon$):** Time API returns an interval $[t.earliest, t.latest]$ where $t.latest - t.earliest = 2\epsilon$.
2. **Commit Wait Principle:** The coordinator assigns a commit timestamp $s = \text{now}().latest$, then delays releasing locks until $\text{now}().earliest > s$.
3. **Lock-Free Read-Only Transactions:** Read queries require zero locks. They read point-in-time data from snapshot MVCC engines across global replicas without coordination overhead.
