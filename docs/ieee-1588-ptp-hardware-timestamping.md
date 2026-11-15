# IEEE 1588 Precision Time Protocol (PTP) & Hardware Timestamping

IEEE 1588 PTP achieves sub-nanosecond clock synchronization across distributed nodes by capturing packet transit timestamps directly in the physical layer (PHY) transceiver hardware.

```mermaid
sequenceDiagram
    participant Master as Grandmaster Clock (GPS Locked)
    participant Slave as Slave Clock (NIC Hardware PHC)

    Master->>Slave: Sync Message (HW Timestamp T1)
    Slave->>Slave: Record HW Ingress Timestamp T2
    Master->>Slave: Follow_Up Message (Sends T1 value)
    Slave->>Master: Delay_Req Message (HW Egress Timestamp T3)
    Master->>Slave: Delay_Resp Message (HW Ingress Timestamp T4)
    Note over Slave: Offset = ((T2 - T1) - (T4 - T3)) / 2
    Note over Slave: Delay = ((T2 - T1) + (T4 - T3)) / 2
```

---

## 1. Socket Configuration Flags

```c
// Enable hardware timestamping on socket
int flags = SOF_TIMESTAMPING_TX_HARDWARE |
            SOF_TIMESTAMPING_RX_HARDWARE |
            SOF_TIMESTAMPING_RAW_HARDWARE;
setsockopt(fd, SOL_SOCKET, SO_TIMESTAMPING, &flags, sizeof(flags));
```
