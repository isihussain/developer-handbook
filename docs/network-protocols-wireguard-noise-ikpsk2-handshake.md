# Network Security: WireGuard Noise_IKpsk2 Handshake Protocol

WireGuard replaces complex VPN protocols with the Noise_IKpsk2 cryptographic protocol, completing secure authenticated mutual key exchange in a single round-trip (1-RTT).

```mermaid
sequenceDiagram
    participant Initiator as Client (Static Key s_i, Ephemeral e_i)
    participant Responder as Server (Static Key s_r, Ephemeral e_r)
    Note over Initiator: Encrypts timestamp under DH(e_i, s_r) and DH(s_i, s_r)
    Initiator->>Responder: Message 1: Initiation (e_i, encrypted s_i, MAC1, MAC2)
    Note over Responder: Computes DH(e_r, e_i) and DH(e_r, s_i) + PSK
    Responder->>Initiator: Message 2: Response (e_r, encrypted empty, MAC1, MAC2)
    Note over Initiator,Responder: Both derive symmetric keys: (Sending Key, Receiving Key)
    Initiator->>Responder: Encrypted Data Packet (Transport Header)
```

---

## 1. State Machine & Denial-of-Service Defense

1. **Passive Identity Hiding:** Initiator static public key is encrypted on the wire; external eavesdroppers cannot determine participant identities.
2. **Cookie Reply (`MAC2`):** Under high CPU load or SYN floods, the responder sends back a tiny stateless cookie rather than performing expensive curve operations, defeating DoS attacks.
3. **Re-Keying Limits:** Sessions re-key automatically every 2 minutes or after transmitting $2^{64} - 2^{13} - 1$ transport messages.
