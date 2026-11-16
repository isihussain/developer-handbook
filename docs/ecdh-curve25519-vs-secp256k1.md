# Applied Cryptography: Curve25519 vs Secp256k1 Architecture

Elliptic Curve Diffie-Hellman (ECDH) establishes shared secret keys over insecure channels. Curve25519 (Montgomery curve) was engineered to prevent timing side channels.

```mermaid
sequenceDiagram
    participant Alice
    participant Bob

    Note over Alice: Generates private key a -> Public Key A = a * G
    Note over Bob: Generates private key b -> Public Key B = b * G
    Alice->>Bob: Transmits Public Key A
    Bob->>Alice: Transmits Public Key B
    Note over Alice: Shared Secret S = a * B
    Note over Bob: Shared Secret S = b * A
    Note over Alice,Bob: S is identical! Hash(S) becomes symmetric AES/ChaCha key.
```

---

## 1. SafeCurves Principles (Bernstein)
- Constant-time ladder step (Montgomery ladder) immune to cache-timing attacks.
- Twist-security prevents small-subgroup invalid curve attacks without explicit point validation.
