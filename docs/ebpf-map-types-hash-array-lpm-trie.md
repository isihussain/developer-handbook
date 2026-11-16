# eBPF Internals: Map Data Structures Architecture

eBPF programs share state across invocations and communicate with userspace via in-kernel Map primitives.

```mermaid
flowchart TD
    BPF_Program[eBPF Kernel Program] --> MapRouter{Map Type Selection}
    MapRouter -->|Exact Key Match| Hash[BPF_MAP_TYPE_HASH: O(1) average lookup]
    MapRouter -->|Fixed Integer Index| Array[BPF_MAP_TYPE_ARRAY: Zero-lock contiguous memory read]
    MapRouter -->|CIDR / Longest Prefix| LPM[BPF_MAP_TYPE_LPM_TRIE: Radix tree for IP routing lookups]
    MapRouter -->|Per-CPU Isolation| PerCPU[BPF_MAP_TYPE_PERCPU_ARRAY: Lockless per-core counters]
```

---

## 1. Concurrency Controls
- `bpf_spin_lock`: Spinlocks protecting map values against concurrent eBPF executions on multi-core systems.
- Atomic operations: `__sync_fetch_and_add` for lockless counter increments.
