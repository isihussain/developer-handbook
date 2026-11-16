# Linux Kernel: Dynamic Kernel Module Support (DKMS) & Taint Flags

When out-of-tree or proprietary drivers (NVIDIA, ZFS) are loaded, the kernel sets specific bit flags in `/proc/sys/kernel/tainted`.

```mermaid
flowchart TD
    ModProbe[modprobe proprietary_driver.ko] --> CheckModule{Is Module Signed & In-Tree?}
    CheckModule -->|Proprietary License| TaintP[Bit 0: TAINT_PROPRIETARY_MODULE (P)]
    CheckModule -->|Force Loaded| TaintF[Bit 1: TAINT_FORCED_MODULE (F)]
    CheckModule -->|Kernel Memory Corrupted| TaintM[Bit 9: TAINT_BAD_PAGE (M)]
```

---

## 1. Upstream Support Invariant
Kernel panic bug reports with `P` or `F` taints are generally rejected by upstream Linux maintainers because closed-source drivers could corrupt arbitrary kernel memory.
