# Signal Double Ratchet: End-to-End Cryptographic Protocol

The Double Ratchet protocol combines a Diffie-Hellman ratchet with two symmetric key-derivation KDF chains, providing both Forward Secrecy and Break-in Recovery (Post-Compromise Security) for asynchronous messaging.

```mermaid
flowchart TD
    DHRatchet[Diffie-Hellman Ratchet: Step on every message round trip] --> RootKey[Root KDF Chain]
    RootKey --> ChainKeys[Sending / Receiving Chain Keys]
    ChainKeys --> SymmetricRatchet[Symmetric KDF Chain: Step on every single message]
    SymmetricRatchet --> MessageKey[Unique Ephemeral Message Key: AES-256-GCM]
```

---

## 1. Security Properties

* **Forward Secrecy:** Past messages cannot be decrypted even if current keys are compromised.
* **Break-in Recovery:** An attacker who compromises the current state loses the ability to decrypt future messages as soon as the participants complete an uncompromised DH ratchet step.
