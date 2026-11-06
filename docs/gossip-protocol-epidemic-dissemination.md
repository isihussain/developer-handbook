# Epidemic Gossip Protocols & SWIM Failure Detection

Gossip protocols achieve decentralized membership tracking, event dissemination, and failure detection with $O(\log N)$ convergence.

```mermaid
flowchart TD
    NodeA[Node A] -->|Ping| NodeB[Node B]
    NodeB -.->|No ACK within timeout| NodeA
    NodeA -->|Ping-Req via K helpers| Helpers[Helper Nodes]
    Helpers -->|Indirect Ping| NodeB
    NodeB -->>|Indirect ACK| Helpers
    Helpers -->>|Relay ACK| NodeA
```

---

## 1. SWIM Protocol Guarantees

- **Constant Message Overhead**: Each node periodically pings a randomly selected peer.
- **Indirect Probing (Ping-Req)**: Avoids false positives from temporary route packet drops.
- **Suspect Mechanism**: Nodes enter `Suspect` status for a configurable period before being declared `Dead`, allowing recovery from GC pauses.
