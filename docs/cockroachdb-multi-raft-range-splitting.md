# CockroachDB: Multi-Raft & Range Splitting Architecture

CockroachDB partitions a single unified key-value space into contiguous 64 MB chunks called Ranges, each managed by its own independent Raft consensus group.

```mermaid
flowchart LR
    subgraph RangeA[Range 1: Keys [/A .. /M)]
        R1_N1[Node 1: Leader] --- R1_N2[Node 2: Follower] --- R1_N3[Node 3: Follower]
    end
    subgraph RangeB[Range 2: Keys [/M .. /Z)]
        R2_N2[Node 2: Leader] --- R2_N3[Node 3: Follower] --- R2_N4[Node 4: Follower]
    end
    RangeA -.->|Automatic split when range reaches 64MB| RangeB
```

---

## 1. Operational Invariants

1. **Automatic Splits:** When a range grows past configured size thresholds (typically 64MB), the leader initiates an atomic split transaction without service interruption.
2. **Rebalancing:** Range leases and replicas rebalance across nodes and availability zones based on disk space, CPU load, and geographic latency rules.
