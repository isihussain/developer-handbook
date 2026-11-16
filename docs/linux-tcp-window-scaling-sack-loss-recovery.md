# TCP Internals: RFC 7323 Window Scaling and RFC 2018 SACK

Standard 16-bit TCP header limits receive window to 64KB. Window Scaling extends maximum window size to 1GB, essential for long-fat networks (LFN).

```mermaid
sequenceDiagram
    participant Sender
    participant Receiver

    Note over Sender,Receiver: Handshake negotiates Window Scale factor 7 (Multiply window by 128)
    Sender->>Receiver: Packets 1, 2, 3, 4, 5
    Note over Receiver: Packet 3 dropped in transit!
    Receiver-->>Sender: SACK Option: Received [1-2] and [4-5]
    Note over Sender: Selective Retransmit: Sender retransmits ONLY Packet 3!
    Note over Sender,Receiver: Packets 4 and 5 are NOT redundantly retransmitted!
```

---

## 1. Throughput Formula
$$\text{Max Throughput} = \frac{\text{Window Size}}{\text{RTT}}$$
With a 64KB window on a 100ms transatlantic link, max speed is capped at 5.2 Mbps regardless of 10Gbps fiber capacity. Window scaling removes this ceiling.
