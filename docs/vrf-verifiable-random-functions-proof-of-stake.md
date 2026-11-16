# Cryptographic Consensus: Verifiable Random Functions (VRF)

A Verifiable Random Function (VRF) generates a pseudorandom number along with a cryptographic proof that only the private key holder could have generated it for a specific input.

```mermaid
sequenceDiagram
    participant Leader as Block Producer (Secret Key SK)
    participant Network as Distributed Validators (Public Key PK)

    Note over Leader: (RandomValue, Proof) = VRF_Hash(SK, SlotNumber)
    Leader->>Network: Broadcast New Block + SlotNumber + Proof
    Note over Network: Verified = VRF_Verify(PK, SlotNumber, RandomValue, Proof)
    Note over Network: Validates leader was legitimately chosen without revealing private seed!
```

---

## 1. Key Invariant
Prevents grinding attacks: The block producer cannot manipulate or predict future lottery outcomes without changing its fixed public key.
