# Linux Power & Latency: CPU Frequency Scaling Governors

CPU frequency governors regulate processor clock speeds and power states based on workload intensity.

```mermaid
flowchart LR
    Governor[cpufreq Governor] --> Choice{Governor Setting}
    Choice -->|performance| MaxClock[Always Pin Core to Max Turbo Frequency: 0 latency transition]
    Choice -->|schedutil| CFS_Util[Integrates directly with CFS scheduler load metrics: 1-2ms transition]
    Choice -->|powersave| MinClock[Pins Core to Lowest Supported Frequency]
```

---

## 1. Production Recommendation
For ultra-low latency databases and network gateways, set:
```bash
echo performance | tee /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor
```
