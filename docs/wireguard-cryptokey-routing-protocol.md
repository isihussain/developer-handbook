# WireGuard Cryptokey Routing & Protocol Architecture

WireGuard instantiates the `Noise_IKpsk2_25519_ChaChaPoly_BLAKE2s` protocol framework, binding public keys directly to permitted IP tunnels inside the Linux network stack.

```mermaid
sequenceDiagram
    participant Initiator as Client (Static i_pk)
    participant Responder as Server (Static r_pk)
    Note over Initiator: Handshake Initiation (148 bytes)
    Initiator->>Responder: msg1: ephemeral e_i, encrypted static i_pk, auth tag, timestamp
    Note over Responder: Handshake Response (92 bytes)
    Responder->>Initiator: msg2: ephemeral e_r, encrypted empty payload, auth tag
    Note over Initiator,Responder: Derive symmetric keys via Blake2s KDF
    Initiator->>Responder: Data Packet (Encrypted ChaCha20-Poly1305)
```

---

## 1. Cryptokey Routing Table

Instead of binding VPN sessions to stateful virtual interfaces:
* The kernel matches destination IP against a routing table: `10.0.0.2/32 -> Peer Public Key PubK_A`.
* Encrypts packet using symmetric key derived from `PubK_A`.
* Enforces firewall rules strictly prior to packet ingress into the host network stack.
