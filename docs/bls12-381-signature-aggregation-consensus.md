# Consensus Cryptography: BLS12-381 Signature Aggregation

Boneh-Lynn-Shacham (BLS) signatures over pairing-friendly elliptic curves allow thousands of validator signatures to be aggregated into a single 48-byte point.

```mermaid
sequenceDiagram
    participant Val1 as Validator 1: sig1
    participant Val2 as Validator 2: sig2
    participant ValN as Validator N: sigN
    participant Aggregator
    participant Verifier

    Val1->>Aggregator: Signature 1
    Val2->>Aggregator: Signature 2
    ValN->>Aggregator: Signature N
    Note over Aggregator: Aggregated Sig = sig1 + sig2 + ... + sigN (Point Addition on G1)
    Aggregator->>Verifier: Send Single Aggregated Signature (48 bytes!)
    Note over Verifier: Single pairing equation verifies all N validators!
```

---

## 1. The Pairing Check
$$e(\sigma_{agg}, g_2) = \prod_{i=1}^{N} e(H(m), pk_i)$$
Enables Ethereum Proof-of-Stake to verify 30,000+ validator votes per 12-second slot with minimal bandwidth overhead.
