# Modern Protocols: QUIC (HTTP/3) Connection Migration Architecture

Traditional TCP connections break when a mobile device switches from Wi-Fi to cellular because the 4-tuple (`saddr, sport, daddr, dport`) changes. QUIC uses a persistent 64-bit Connection ID (CID).

```mermaid
sequenceDiagram
    participant Mobile as Client (Mobile Phone: CID 0xABC)
    participant Server as Web Server (HTTP/3)

    Mobile->>Server: Wi-Fi: IP 192.168.1.5 -> Data transfer active
    Note over Mobile: Switches to 5G Cellular! New IP: 172.56.20.12
    Mobile->>Server: Cellular: IP 172.56.20.12 (Same CID: 0xABC!)
    Server->>Server: Path Validation (PATH_CHALLENGE / PATH_RESPONSE)
    Server-->>Mobile: Connection Migrated Seamlessly without Handshake!
```

---

## 1. Zero Interruption
Active video streams, WebSockets, and downloads continue uninterrupted during network handoffs.
