# NUMA Memory Balancing & Automatic Page Migration

Non-Uniform Memory Access systems partition physical RAM across CPU sockets. The kernel balancer samples unmapped pages to dynamically migrate memory closer to the consuming CPU node.

```mermaid
flowchart LR
    subgraph Socket 0 - NUMA Node 0
        CPU0[CPU Cores 0-31]
        RAM0[Local RAM: Low Latency]
    end
    subgraph Socket 1 - NUMA Node 1
        CPU1[CPU Cores 32-63]
        RAM1[Local RAM: Low Latency]
    end
    CPU0 -->|Fast Local Access ~80ns| RAM0
    CPU0 -->|Slow Remote UPI/QPI Interconnect ~140ns| RAM1
    Kernel[AutoNUMA Balancer] -.->|Unmap & Detect Fault| Page[Page Migration to Local Node]
```

---

## 1. Kernel Optimization Flags

```bash
# Verify NUMA topology and locality hit rates
numactl --hardware
numastat -c

# Enforce strict node memory allocation
numactl --cpunodebind=0 --membind=0 ./high-perf-app
```
