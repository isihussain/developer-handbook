# QUIC Connection Migration & 0-RTT Handshake Architecture

QUIC replaces 4-tuple IP/Port bindings with cryptographic 64-bit Connection IDs (CIDs), allowing active TLS sessions to survive network transitions seamlessly without renegotiation.

```mermaid
sequenceDiagram
    participant Client as Mobile Client (WiFi)
    participant Server as QUIC Server
    Client->>Server: Packets over WiFi (IP: 192.168.1.50, CID: 0x8a9f)
    Server-->>Client: Response (CID: 0x8a9f)
    Note over Client: Interface switches to LTE (IP: 100.64.0.12)
    Client->>Server: PATH_CHALLENGE Packet over LTE (CID: 0x9b10)
    Server-->>Client: PATH_RESPONSE Packet
    Note over Client,Server: Connection Migrated Zero Disruption
```

---

## 1. 0-RTT Anti-Replay Defense

1. Client caches TLS session ticket containing cryptographic parameters from previous session.
2. Initial flight sends encrypted early data alongside `ClientHello`.
3. Server rejects replayed 0-RTT requests via strike registers or monotonically increasing transaction tokens.
