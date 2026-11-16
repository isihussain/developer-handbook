# High-Performance Networking: DPDK Poll Mode Driver Architecture

Intel Data Plane Development Kit (DPDK) bypasses the Linux kernel entirely, assigning physical NIC hardware rings directly to userspace polling threads.

```mermaid
flowchart LR
    NIC[100GbE NIC RX Ring] --> PMD[DPDK Poll Mode Driver (PMD): Continuous Hardware Polling]
    PMD --> Hugepages[DPDK Hugepage Memory Pool (rte_mempool)]
    Hugepages --> PacketProcess[Packet Processing Pipeline: 100M packets/sec!]
    subgraph Zero_Interrupts
        PMD -.->|Zero Hardware Interrupts| CPU[Dedicated 100% Core Pinning]
    end
```

---

## 1. Tradeoffs
- **Pros:** Maximum possible throughput (148 million packets per second per 100GbE port).
- **Cons:** Dedicated CPU core runs at 100% utilization continuously polling the ring.
