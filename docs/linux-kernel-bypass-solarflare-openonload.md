# High-Frequency Trading: OpenOnload User-Level Network Stack

OpenOnload intercepts standard socket calls (`socket`, `read`, `write`) via `LD_PRELOAD` and runs a full TCP/IP stack directly inside userspace application memory.

```mermaid
flowchart TD
    App[Trading Application: Standard BSD Sockets] --> Onload[Onload Userspace TCP/IP Stack]
    Onload --> DirectHW[Direct Hardware Ring Buffer Access via PCIe Bar]
    DirectHW --> NIC[Solarflare SmartNIC Hardware]
    subgraph Bypass
        Onload -.->|Completely Bypasses!| LinuxKernel[Linux Kernel Network Stack: 0 Syscalls & 0 Context Switches!]
    end
```

---

## 1. Latency Profile
Reduces half-round-trip TCP latency from ~4-6 microseconds down to ~700-900 nanoseconds.
