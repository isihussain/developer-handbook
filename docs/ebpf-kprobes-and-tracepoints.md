# Dynamic Tracing: eBPF kprobes vs Static Kernel Tracepoints

eBPF programs attach to kernel functions dynamically using software breakpoint injection or static instrumentation tracepoints.

---

## 1. Comparison

| Feature | `kprobe` / `kretprobe` | Static `tracepoint` |
|---|---|---|
| **Mechanism** | Dynamic instruction replacement (`int3` / `brk` trap). | Hardcoded static tracepoints macro in kernel C source. |
| **Stability** | Fragile across kernel versions (symbols change). | Guaranteed stable ABI across kernel upgrades. |
| **Overhead** | Higher due to instruction patching and register push/pop. | Zero overhead when disabled; simple jump when active. |
| **Argument Access** | Manual register offset extraction via `PT_REGS_PARM`. | Type-safe struct fields defined in tracepoint format. |
