# Monero RingCT: Ring Signatures & Pedersen Commitments

Ring Confidential Transactions (RingCT) obscure transaction amounts and sender identity by pairing homomorphic Pedersen commitments with MLSAG ring signatures.

```mermaid
flowchart TD
    Amount[Transaction Amount v, blinding factor r] --> Commit[Pedersen Commitment: C = r * G + v * H]
    Commit --> Ring[Ring Signature: Mix real input with decoy public keys]
    Ring --> KeyImage[Key Image: I = x * H_p P - Prevents Double Spending]
    Commit --> RangeProof[Bulletproofs Range Proof: Proves 0 <= v < 2^64 without revealing v]
```

---

## 1. Homomorphic Balance Invariant

$$\sum C_{	ext{inputs}} - \sum C_{	ext{outputs}} = \left(\sum r_{	ext{in}} - \sum r_{	ext{out}}ight) G + \left(\sum v_{	ext{in}} - \sum v_{	ext{out}}ight) H$$
When $\sum v_{	ext{in}} = \sum v_{	ext{out}}$, the $H$ term vanishes, proving balance without exposing values.
