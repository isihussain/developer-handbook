# Linux Network Stack: Softirq Processing Budget and NAPI Polling

Network packet ingress uses the New API (NAPI) polling loop to prevent interrupt storms from overwhelming the host CPU.

```mermaid
sequenceDiagram
    participant NIC as Network Interface Card
    participant HardIRQ as Top-Half (Hardware IRQ)
    participant SoftIRQ as Bottom-Half (NET_RX_SOFTIRQ)
    participant Kernel as ksoftirqd/N Worker Thread

    NIC->>HardIRQ: Packet Arrival: Hardware Interrupt
    Note over HardIRQ: Disables NIC interrupts on that RX queue!
    HardIRQ->>SoftIRQ: Schedules NET_RX_SOFTIRQ
    Note over SoftIRQ: net_rx_action() polls up to net.core.netdev_budget (default 300) packets
    Note over SoftIRQ: Budget exhausted or time limit exceeded!
    SoftIRQ->>Kernel: Wakes ksoftirqd/N to finish processing in background
    Note over Kernel: Re-enables NIC hardware interrupts
```

---

## 1. Tuning Packet Drops
When `/proc/net/softnet_stat` column 3 increments, packets are being dropped due to budget exhaustion. Increase `net.core.netdev_budget = 600`.
