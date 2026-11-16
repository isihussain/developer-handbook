# Network Hardening: TCP SYN Cookie State-Free Handshakes

Under SYN flood attacks, the Linux kernel TCP listen backlog overflows. SYN cookies eliminate the half-open state table by encoding connection state into the Initial Sequence Number (ISN).

```mermaid
sequenceDiagram
    participant Attacker as Attacker (SYN Flood)
    participant Kernel as Linux Kernel (net.ipv4.tcp_syncookies = 1)

    Attacker->>Kernel: TCP SYN (Spoofed IP)
    Note over Kernel: Listen queue FULL! Do NOT allocate socket control block!
    Note over Kernel: ISN = Hash(saddr, daddr, sport, dport, secret, timestamp)
    Kernel-->>Attacker: SYN-ACK with Cookie ISN
    Note over Attacker: Attacker never sends ACK -> 0 bytes memory consumed on server!
```

---

## 1. Cryptographic Invariants
When a legitimate client responds with final ACK, the acknowledgment number is `ISN + 1`. The kernel recomputes the hash; if verified, it instantiates the socket struct on demand.
