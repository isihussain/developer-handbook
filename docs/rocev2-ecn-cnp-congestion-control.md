# RoCEv2 ECN & Congestion Notification Packets (CNP) Architecture

RDMA over Converged Ethernet v2 uses Explicit Congestion Notification (ECN) and Congestion Notification Packets (CNP) to eliminate packet loss and buffer overflow without relying solely on coarse Priority Flow Control (PFC) pause frames.

```mermaid
sequenceDiagram
    participant Sender as RoCEv2 Sender
    participant Switch as Top-of-Rack Switch
    participant Receiver as RoCEv2 Receiver

    Sender->>Switch: RDMA Write Packet (IP Header: ECN = ECT 01)
    Note over Switch: Switch egress queue exceeds ECN threshold!
    Switch->>Receiver: Forwards RDMA Packet with CE flag set (ECN = 11)
    Receiver->>Sender: Sends High-Priority CNP (Congestion Notification Packet)
    Sender->>Sender: Throttle transmission rate according to DCQCN state machine
```

---

## 1. DCQCN Algorithm

* **Rate Reduction:** When sender receives CNP, target rate $R_T$ updates:
  $$R_T = R_C, \quad R_C = R_C \cdot (1 - lpha / 2)$$
* **Rate Recovery:** Sender recovers rate periodically via additive increase timers when no CNPs arrive.
