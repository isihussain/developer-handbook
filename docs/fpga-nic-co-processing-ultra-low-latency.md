# FPGA-NIC Co-Processing & Bump-in-the-Wire Architecture

FPGA-accelerated SmartNICs parse market data ticks in silicon and transmit orders via bump-in-the-wire logic in under 50 nanoseconds.

```mermaid
flowchart LR
    OpticalLine[10G/25G Optical Ingress] --> MAC[Hardware MAC / PHY]
    MAC --> Parser[FPGA Market Data Parser: 18 ns]
    Parser --> Trigger[FPGA Pre-Trade Risk & Trigger Logic: 12 ns]
    Trigger -->|Direct Order Emit| OutboundMAC[Hardware TX Engine: 15 ns]
    OutboundMAC --> Exchange[Optical Outbound to Exchange]
    Trigger -.->|PCIe DMA Notification| HostCPU[Host Software Log / Replay]
```

---

## 1. Tick-to-Trade Latency Breakdown

| Subsystem Component | Software (Optimized C++) | FPGA Bump-in-the-Wire |
|---|---|---|
| Optical PHY Ingress | ~12 ns | ~12 ns |
| Packet Parsing & Deserialization | 80 ns – 200 ns | 15 ns – 25 ns |
| Price Comparison & Trading Decision | 50 ns – 150 ns | 10 ns – 20 ns |
| Outbound Order Serialization | 60 ns – 150 ns | 12 ns – 18 ns |
| PCIe Transfer to NIC | 250 ns – 450 ns | **0 ns** (Executed directly on NIC) |
| **Total Tick-to-Trade** | **452 ns – 962 ns** | **49 ns – 75 ns** |
