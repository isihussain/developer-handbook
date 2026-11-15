# Tor Onion Routing: Telescopic Circuit Cryptography Architecture

Tor routes traffic through a three-hop relay circuit (Guard, Middle, Exit), wrapping application packets in multiple concentric layers of AES/ChaCha20 encryption using Diffie-Hellman key exchanges.

```mermaid
sequenceDiagram
    participant Client
    participant Guard as Guard Node
    participant Middle as Middle Relay
    participant Exit as Exit Node

    Client->>Guard: CREATE2 (ntor DH Handshake with Guard) -> Shared Key K1
    Client->>Guard: EXTEND2 (encrypted with K1) -> Handshake with Middle -> Shared Key K2
    Client->>Guard: EXTEND2 (encrypted with K1, K2) -> Handshake with Exit -> Shared Key K3
    Note over Client: Circuit Built. Outgoing cell encrypted: E_K1(E_K2(E_K3(Payload)))
    Client->>Guard: Sends 3-layer encrypted Cell
    Guard->>Middle: Strips Layer 1 (K1)
    Middle->>Exit: Strips Layer 2 (K2)
    Exit->>Destination: Strips Layer 3 (K3) -> Forwards plaintext to Web
```

---

## 1. Forward Secrecy & Cell Sizing

All relay payloads are packed into fixed 514-byte cells (`RelayCell`), preventing traffic analysis and timing correlation attacks across intermediate routing hops.
