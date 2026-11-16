# Kernel Tuning: CPU Affinity and Core Isolation (isolcpus)

Context switches and CPU core migrations invalidate L1/L2 hardware caches, introducing tail-latency spikes in high-frequency trading and low-latency engines.

```mermaid
flowchart TD
    KernelBoot["Kernel Boot Argument: isolcpus=2,3 nohz_full=2,3 rcu_nocbs=2,3"] --> Cores23[Cores 2 & 3 Isolated from OS Scheduler]
    App[Low-Latency Process] --> Pin["taskset -c 2 ./trading_engine"]
    Pin --> Core2[Runs Exclusively on Core 2: Zero OS Interrupts & Zero Cache Eviction!]
```

---

## 1. Companion Flags
- `nohz_full`: Disables scheduling tick on specified cores when only 1 runnable task exists.
- `rcu_nocbs`: Offloads RCU callback processing from isolated cores to housekeeping cores.
