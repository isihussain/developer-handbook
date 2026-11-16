# Hardware Topology: Linux NUMA Architecture and Memory Locality

Non-Uniform Memory Access (NUMA) multi-socket systems feature local RAM with low latency and remote socket RAM with higher interconnect overhead (UPI/QPI).

```mermaid
flowchart LR
    CPU0[Socket 0 CPU Core] -->|Fast: 60ns| Node0_RAM[(Node 0 Local Memory)]
    CPU0 -.->|Slow Interconnect: 140ns| Node1_RAM[(Node 1 Remote Memory)]
    Kernel[Automatic NUMA Balancing] -->|Page Fault Unhinted| Migrate[Migrate Page to Node 0]
```

---

## 1. Memory Policies
- `MPOL_BIND`: Strictly allocates from specified NUMA nodes only.
- `MPOL_INTERLEAVE`: Alternates allocations round-robin across nodes (common for shared memory caches).
