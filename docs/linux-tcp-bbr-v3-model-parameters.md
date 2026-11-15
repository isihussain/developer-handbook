# TCP: BBRv3 Congestion Control Architecture

BBRv3 (Bottleneck Bandwidth and Round-trip propagation time) regulates sending rates using state machine models of the network path rather than reacting to packet drops like loss-based algorithms.

```mermaid
stateDiagram-v2
    [*] --> Startup: Probe for max bandwidth (Gain 2.89)
    Startup --> Drain: Drain queue when inflight exceeds bandwidth delay product
    Drain --> ProbeBW: Pacing rate matches measured delivery rate
    ProbeBW --> ProbeRTT: Periodically drop inflight to measure true min RTT
    ProbeRTT --> ProbeBW
```

---

## 1. Core Model Parameters

1. **$BtlBw$ (Bottleneck Bandwidth):** Estimated by tracking maximum delivery rate over a sliding time window (typically 10 round trips).
2. **$RTprop$ (Round-Trip Propagation Time):** Measured by recording the minimum observed RTT over a 10-second window during which sending volume is constrained.
3. **BDP (Bandwidth Delay Product):** $BtlBw 	imes RTprop$ defines the exact volume of data that can be in flight without bufferbloat queueing.
