# Kernel Tracing: Tracepoints, Kprobes, and Uprobes Overhead

Dynamic tracing allows deep kernel inspection with eBPF, but choice of instrumentation hook dictates overhead and safety.

```mermaid
flowchart TD
    TraceRequest[Instrumentation Hook] --> Choice{Type of Probe}
    Choice -->|Static Kernel| Tracepoint[Tracepoints: Static inline NOPs: ~15ns overhead]
    Choice -->|Dynamic Kernel| Kprobe[Kprobes: Dynamic breakpoint instruction patch: ~100ns]
    Choice -->|Dynamic Userspace| Uprobe[Uprobes: Userspace trap to kernel context: ~1500ns]
```

---

## 1. Production Guidance
- Prefer static **Tracepoints** (`sched:sched_switch`, `syscalls:sys_enter_*`) in hot paths.
- Avoid high-frequency **Uprobes** (>100k calls/sec) without BPF uprobe-multi optimizations.
