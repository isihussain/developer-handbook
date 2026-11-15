# Cryptography: BIP-340 Schnorr Signatures & Bitcoin Taproot

BIP-340 Schnorr signatures replace ECDSA in modern cryptographic protocols, offering provable security, linear signature aggregation (MuSig2), and batch verification.

```mermaid
flowchart TD
    KeyGen[Key Generation: Secret Key d, Public Key P = d*G] --> Nonce[Deterministic Nonce: k = H(d || m)]
    Nonce --> PointR[Commitment Point: R = k*G]
    PointR --> Challenge[Fiat-Shamir Challenge: e = H(R || P || m)]
    Challenge --> ScalarS[Scalar Signature: s = k + e * d mod n]
    ScalarS --> Signature[Output: (R, s)]
```

---

## 1. Core Advantages over ECDSA

1. **Linearity:** Public keys and signatures can be aggregated linearly:

$$\sum P_i = \left(\sum d_iight) G, \quad \sum s_i = \sum k_i + e \sum d_i$$

2. **Batch Verification:** Verifying $K$ signatures independently requires $2K$ elliptic curve point multiplications. Schnorr batch verification verifies $K$ signatures using only $K + 1$ scalar multiplications.
3. **Taproot Tweaking:** Public keys can be cryptographically tweaked with a Merkle root $T$ of scripts ($P' = P + H(P \parallel T)G$), enabling script trees to hide inside what appears to be a standard single-key output.
