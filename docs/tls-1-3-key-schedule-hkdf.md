# Network Security: TLS 1.3 Key Schedule & HKDF State Machine

TLS 1.3 eliminates obsolete key exchange mechanisms (RSA, static Diffie-Hellman) in favor of an ephemeral HKDF state machine that derives distinct cryptographic keys for handshake, application, and resumption contexts.

```mermaid
flowchart TD
    EarlySecret[Early Secret: HKDF-Extract(0, PSK)] --> HandshakeSecret[Handshake Secret: HKDF-Extract(..., ECDHE Shared Secret)]
    HandshakeSecret --> MasterSecret[Master Secret: HKDF-Extract(..., 0)]
    HandshakeSecret --> CHTS[Client Handshake Traffic Secret]
    HandshakeSecret --> SHTS[Server Handshake Traffic Secret]
    MasterSecret --> CATS[Client Application Traffic Secret]
    MasterSecret --> SATS[Server Application Traffic Secret]
    MasterSecret --> ResumptionSecret[Resumption Master Secret]
```

---

## 1. Key Derivation Functions

1. **`HKDF-Extract(salt, IKM)`:** Extracts a pseudorandom key (PRK) from input keying material.
2. **`HKDF-Expand-Label(Secret, Label, Context, Length)`:** Expands secrets into context-bound keys:

$$	ext{HKDF-Expand-Label} = 	ext{HKDF-Expand}(	ext{Secret}, 	ext{LabelInfo}, L)$$

3. **Forward Secrecy:** Compromise of application traffic secrets cannot compromise prior handshake secrets or future re-keyed application sessions (`KeyUpdate`).
