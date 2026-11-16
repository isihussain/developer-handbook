# eBPF Internals: BPF Ring Buffer (bpf_ringbuf) Architecture

The BPF Ring Buffer (`BPF_MAP_TYPE_RINGBUF`) replaces per-CPU perf buffers with a shared multi-producer single-consumer circular buffer.

```mermaid
flowchart LR
    subgraph Legacy_Perf_Buffer
        CPU0[CPU 0 Perf Buffer]
        CPU1[CPU 1 Perf Buffer]
        CPU2[CPU 2 Perf Buffer: OVERFLOW!]
    end
    subgraph BPF_Ring_Buffer
        AllCPUs[All CPU Cores] -->|Lockless Atomic Reservation| SharedRing[Single Shared Memory Ring Buffer]
        SharedRing --> Consumer[Single Userspace Consumer Polls 1 FD!]
    end
```

---

## 1. Advantages of Ringbuf
- **Memory Efficiency:** Eliminates per-CPU sizing imbalances where busy cores drop events while idle cores waste memory.
- **Strict In-Order Event Ordering:** Events across all CPU cores are ordered chronologically by submission time.
