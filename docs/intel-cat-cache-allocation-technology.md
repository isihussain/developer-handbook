# Intel RDT: Cache Allocation Technology (CAT) & Memory Bandwidth

Intel Cache Allocation Technology (CAT) partitions the shared Last Level Cache (L3 LLC) into dedicated hardware bitmasks to eliminate "noisy neighbor" cache thrashing.

```mermaid
flowchart LR
    subgraph L3_Cache[Shared L3 Last Level Cache: 16 Bit Ways]
        HighPri[COS 1: High Priority - Bits 0000_1111_1111_1111 (75% LLC)]
        LowPri[COS 2: Batch Processing - Bits 1111_0000_0000_0000 (25% LLC)]
    end
    HFTApp[Latency-Critical Trading App] --> HighPri
    BatchApp[Background Compression Worker] --> LowPri
```

---

## 1. Class of Service (COS) Management

* **Hardware MSRs:** CPUs expose Model-Specific Registers (`IA32_L3_MASK_n`) mapping Class of Service identifiers to cache-way bitmasks.
* **Linux `resctrl` Subsystem:** The kernel mounts `/sys/fs/resctrl`, allowing administrators to assign process PIDs to dedicated cache and memory bandwidth schemas without restarting workloads.
