# Data Architecture: Cache Invalidation Patterns and Consistency

Choosing between Cache-Aside, Write-Through, and Write-Behind dictates data consistency, write latency, and database pressure.

```mermaid
flowchart TD
    Pattern -->|Cache-Aside (Lazy)| CA["App reads Cache. Miss -> reads DB -> writes Cache. App writes directly to DB & invalidates Cache."]
    Pattern -->|Write-Through| WT["App writes to Cache. Cache synchronously writes to DB before acknowledging. Strong consistency!"]
    Pattern -->|Write-Behind (Write-Back)| WB["App writes to Cache. Cache acknowledges immediately & asynchronously flushes to DB. High throughput!"]
```

---

## 1. Tradeoffs
- **Cache-Aside:** Most resilient; cache failure does not crash the system.
- **Write-Behind:** Highest write throughput; potential data loss if cache nodes crash before flushing to database.
