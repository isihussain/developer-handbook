# Peer-to-Peer Networking: libp2p Yamux Stream Multiplexer

libp2p utilizes stream multiplexers to open arbitrary bidirectional channels over a single secure connection (Noise/TLS).

```mermaid
flowchart LR
    AppStream1[Kademlia DHT Stream] --> Yamux[Yamux Multiplexer: Window-Based Flow Control]
    AppStream2[Bitswap Block Stream] --> Yamux
    AppStream3[Gossipsub Message Stream] --> Yamux
    Yamux --> Transport[Single Encrypted TCP / QUIC Connection]
```

---

## 1. Yamux vs mplex
- **mplex:** Minimal framing without flow control; prone to buffer overflows on heterogeneous peers.
- **Yamux (Yet Another Multiplexer):** RFC-grade window-based flow control preventing slow readers from stalling the transport connection.
