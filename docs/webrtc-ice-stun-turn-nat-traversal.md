# Real-Time Communication: Interactive Connectivity Establishment (ICE)

WebRTC establishes peer-to-peer audio/video streaming across firewalls and symmetric NATs using the ICE framework.

```mermaid
sequenceDiagram
    participant PeerA as Peer A (Behind NAT)
    participant STUN as STUN Server (Public IP:Port Discovery)
    participant TURN as TURN Relay (Symmetric NAT Fallback)
    participant PeerB as Peer B

    PeerA->>STUN: Binding Request
    STUN-->>PeerA: Public Mapped Address: 203.0.113.5:54321 (srflx candidate)
    PeerA->>PeerB: Exchange SDP Offers with ICE Candidates
    Note over PeerA,PeerB: Direct P2P UDP connection established!
    Note over PeerA,PeerB: If Symmetric NAT blocks direct path -> Relays over TURN server!
```

---

## 1. Candidate Priority
ICE attempts candidates in strict cost order: Host (local LAN) $\rightarrow$ Server Reflexive (STUN) $\rightarrow$ Relay (TURN).
