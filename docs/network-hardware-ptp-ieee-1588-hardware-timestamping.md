# High-Precision Timing: IEEE 1588 PTP & Hardware PHY Timestamping

Precision Time Protocol (IEEE 1588 PTP / IEEE 802.1AS) synchronizes distributed clocks with sub-microsecond accuracy by recording hardware timestamps directly at the physical Ethernet PHY transceiver layer, bypassing OS kernel stack jitter.

```mermaid
sequenceDiagram
    participant Master as Master Clock (Grandmaster)
    participant Slave as Slave Clock (PHC)
    Master->>Slave: Sync Message (Master records Hardware egress t1)
    Master->>Slave: Follow_Up Message (Carries precise timestamp t1)
    Note over Slave: Slave records Hardware ingress t2
    Slave->>Master: Delay_Req Message (Slave records Hardware egress t3)
    Note over Master: Master records Hardware ingress t4
    Master->>Slave: Delay_Resp Message (Carries precise timestamp t4)
    Note over Slave: Computes Mean Path Delay & Clock Offset
```

---

## 1. Calculation Equations

$$	ext{One-Way Delay} = rac{(t_4 - t_1) - (t_3 - t_2)}{2}$$

$$	ext{Clock Offset} = rac{(t_2 - t_1) - (t_4 - t_3)}{2}$$

* **PTP Hardware Clock (PHC):** Linux exposes hardware clocks via `/dev/ptpX`. The `phc2sys` daemon synchronizes the system clock (`CLOCK_REALTIME`) to the hardware PHC device.
