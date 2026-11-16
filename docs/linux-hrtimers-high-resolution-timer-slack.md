# Linux Kernel: hrtimer Subsystem and Timer Slack Optimization

Legacy kernel timers operate on low-frequency ticks (`CONFIG_HZ` = 250 or 1000). The `hrtimer` subsystem provides nanosecond timer resolution backed by hardware APIC TSC counters.

```mermaid
flowchart TD
    TimerApp[Application Sleep Request: nanosleep 500us] --> HRTimer[hrtimer Subsystem]
    HRTimer --> Slack{"Apply Timer Slack (/proc/sys/fs/timer_slack_ns)?"}
    Slack -->|Group Timers Together| CPU_Wake[Wake CPU Core ONCE for Multiple Timers: Conserves Power]
    Slack -->|Zero Slack| ExactWake[Wake CPU Core at Exact Nanosecond]
```

---

## 1. Latency Sensitive Tuning
For real-time trading or trading gateways, set timer slack to 1ns via `prctl(PR_SET_TIMERSLACK, 1)`.
