# High-Performance Networking: TCP BBR Congestion Control

Google BBR (Bottleneck Bandwidth and RTT) models the physical network pipe rather than treating packet loss as a congestion indicator like legacy Cubic.

```mermaid
flowchart LR
    ProbeBW[ProbeBW: Estimates Max Ingress Bandwidth] --> BDP[Calculate BDP = BtlBw * RTprop]
    ProbeRTT[ProbeRTT: Drains queue to measure physical min RTT] --> BDP
    BDP --> Pacing[Set Pacing Rate: Exact Pipe Capacity with 0 Bufferbloat]
```

---

## 1. BDP Principle
By pacing packets at exactly the bottleneck bandwidth, BBR prevents queues from building up in router buffers, eliminating latency bloat on high-throughput networks.
