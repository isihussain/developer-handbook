# InfiniBand Verbs Queue Pair (QP) State Machine Architecture

InfiniBand and RoCE communicate using Queue Pairs (Send Queue + Receive Queue). Transitions follow a strict hardware-enforced lifecycle managed through the `ibv_modify_qp` verbs system interface.

```mermaid
stateDiagram-v2
    [*] --> RESET: ibv_create_qp
    RESET --> INIT: Set port, PKey, access flags
    INIT --> RTR: Set destination LID/GID, QPN, PSN (Ready to Receive)
    RTR --> RTS: Set local PSN, retry counts (Ready to Send)
    RTS --> SQD: Send Queue Drained
    SQD --> RTS: Resume
    RTS --> ERROR: Transmission / Path Failure
    ERROR --> RESET: Re-initialize
```

---

## 1. Queue Pair Types

| Transport Type | Connection Mode | Reliability | Operations Supported |
|---|---|---|---|
| **RC (Reliable Connection)** | 1-to-1 Dedicated Pair | Hardware ACK/NACK | RDMA Read, Write, Atomic, Send/Receive |
| **UC (Unreliable Connection)** | 1-to-1 Dedicated Pair | No retransmission | RDMA Write, Send/Receive |
| **UD (Unreliable Datagram)** | 1-to-Many Multicast | Best effort | Send/Receive only (MTU bounded) |
