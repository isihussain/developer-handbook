# High-Speed Ethernet: Flow Control & Priority Flow Control (PFC)

To achieve lossless transmission for RoCEv2 (RDMA over Converged Ethernet), switches and NICs manage congestion using Priority-Based Flow Control (IEEE 802.1Qbb).

```mermaid
flowchart TD
    subgraph StandardPause[IEEE 802.3x Global PAUSE]
        P1[Switch Ingress Buffer Full] -->|Transmits EtherType 0x8808 PAUSE| P2[Sender NIC Halts ALL Traffic]
        P2 --> HoL[Head-of-Line Blocking Across Unrelated Flows]
    end
    subgraph PFC_8021Qbb[IEEE 802.1Qbb Priority Flow Control]
        PFC1[Queue Buffer for Priority 3 Full] -->|PFC Frame: Pause bit for CoS 3| PFC2[Sender NIC Halts ONLY Priority 3]
        PFC2 --> OtherQueues[Queues 0, 1, 2, 4-7 Continue Transmitting at Line Rate]
    end
```

---

## 1. Deadlock Mitigations

* **PFC Deadlock:** Cyclic dependencies across multi-hop switch fabrics can form PAUSE loops, halting traffic indefinitely.
* **Watchdog Timers:** Modern top-of-rack switches run PFC Watchdogs that monitor buffer queues; if a queue stays in a PAUSE state beyond threshold (e.g., $100\mu	ext{s}$), the switch drops packets to break the deadlock.
