# HTAP: Hybrid Transactional & Analytical Processing Architectures

HTAP database architectures unify OLTP (row-oriented, low latency writes) and OLAP (column-oriented, large scans) workloads without separate ETL pipelines.

```mermaid
flowchart LR
    subgraph RowEngine[Row Store: Raft Leader & Followers (OLTP)]
        NodeA[Leader Node: RocksDB Row Store]
    end
    subgraph ColumnEngine[Columnar Replicas: Vectorized Raft Learners (OLAP)]
        Learner[Learner Node: Vectorized Column Store]
    end
    NodeA -->|Raft Log Streaming: Real-time replication| Learner
    OLTPClient[Transactional Client] --> NodeA
    OLAPClient[Analytical Client] --> Learner
```

---

## 1. Architectural Patterns

* **Dual-Format Replicas (e.g., TiDB / TiFlash):** OLTP writes hit row-based storage nodes. Dedicated learner replicas consume the Raft log asynchronously, converting rows into columnar formats without affecting transaction throughput.
* **CH-benCHmark Standard:** Industry standard for evaluating HTAP systems by running TPC-C transactional workloads concurrently with TPC-H analytical queries against identical live data.
