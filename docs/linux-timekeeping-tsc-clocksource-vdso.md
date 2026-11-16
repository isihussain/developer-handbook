# Microarchitecture: Time Stamp Counter (TSC) and vDSO clock_gettime()

Calling `clock_gettime()` millions of times per second in high-frequency trading causes massive overhead if routed through kernel system calls. vDSO eliminates the syscall.

```mermaid
flowchart LR
    App[clock_gettime(CLOCK_MONOTONIC)] --> vDSO[vDSO: Virtual Dynamic Shared Object in userspace]
    vDSO --> RDTSC[RDTSCP: Direct Hardware CPU Instruction: 15ns!]
    vDSO --> Calibration[Calibrates with kernel memory page: Zero Context Switch!]
```

---

## 1. Invariant: Invariant TSC
Modern x86 CPUs feature Constant and Invariant TSC (`constant_tsc`, `nonstop_tsc`), guaranteeing the counter ticks at the exact same rate regardless of CPU frequency scaling (P-states/C-states).
