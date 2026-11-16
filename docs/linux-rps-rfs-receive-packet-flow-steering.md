# Network Scale: RPS (Receive Packet Steering) & RFS Architecture

Receive Packet Steering (RPS) distributes packet processing across multiple CPU cores for single-queue network cards.

```mermaid
flowchart TD
    SingleQueueNIC[Single RX Queue NIC] --> Driver[Device Driver]
    Driver --> RPS[RPS: Hashes packet 4-tuple to CPU Mask]
    RPS --> Core1[CPU Core 1 SoftIRQ]
    RPS --> Core2[CPU Core 2 SoftIRQ]
    RPS --> Core3[CPU Core 3 SoftIRQ]
    RPS --> RFS[RFS: Steers packet directly to core running application socket!]
```

---

## 1. RFS Cache Locality
Receive Flow Steering (RFS) tracks which CPU core is actively reading the socket, routing packets to that exact core to keep CPU L1/L2 caches hot.
