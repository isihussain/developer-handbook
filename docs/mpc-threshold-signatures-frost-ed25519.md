# Threshold Security: FROST Flexible Round-Optimized Schnorr Signatures

FROST provides $t$-of-$n$ threshold Schnorr signatures (Ed25519) in two network rounds, producing standard signatures indistinguishable from single-signer keys.

```mermaid
sequenceDiagram
    participant P1 as Participant 1 (Share 1)
    participant P2 as Participant 2 (Share 2)
    participant Coord as Signature Coordinator

    Note over P1,P2: Round 1: Pre-process Nonces (Commitments D, E)
    Coord->>P1: Request Signature Share on Message M
    Coord->>P2: Request Signature Share on Message M
    Note over P1,P2: Round 2: Compute Response Share z_i
    P1-->>Coord: z_1
    P2-->>Coord: z_2
    Note over Coord: Aggregates Shares: Single Ed25519 Signature (R, z)!
```

---

## 1. Verification Invariant
External verifiers use standard Ed25519 verification algorithms; they cannot detect that a threshold of independent signers produced the signature.
