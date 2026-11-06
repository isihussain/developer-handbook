# Ring Signatures & Confidential Transactions (RingCT)

Ring Signatures conceal the true signer among a set of decoy public keys, while Confidential Transactions hide transfer amounts using homomorphic commitments and zero-knowledge range proofs.

```mermaid
flowchart TD
    KeyImage[Key Image I = x * H_p(P)] --> Ring[Ring of Public Keys: P_1, P_2, ..., P_n]
    Ring --> SchnorrRing[Cyclic Schnorr Ring Equation: sum e_i = c]
    SchnorrRing --> PedCommit[Pedersen Commitment to Amount: C = v*G + b*H]
    PedCommit --> BalanceCheck[Homomorphic Balance: sum C_in == sum C_out + Fee*G]
    BalanceCheck --> RangeProof[Bulletproofs Range Proof: v in 0..2^64-1]
```

---

## 1. Key Image & Double-Spend Prevention

To prevent double spending when the signer identity is anonymous:
1. True private key $x$ has public key $P = x \cdot G$.
2. Computes deterministic **Key Image**:
   $$I = x \cdot H_p(P)$$
   where $H_p$ hashes to an elliptic curve point.
3. Every transaction publishes $I$.
4. The ledger maintains a set of spent key images. If $I$ appears twice, the second transaction is rejected, without disclosing which ring member signed it.
