# Inner Product Arguments & Bulletproofs

Bulletproofs provide short, transparent zero-knowledge range proofs and verifiable general arithmetic circuits without requiring a trusted setup ceremony.

```mermaid
flowchart TD
    Input[Secret Vector a, Public Vector b, Scalar c = dot(a, b)] --> Init[Commitment P = g^a * h^b * u^c]
    Init --> Halving[Recursive Halving: a_L, a_R, b_L, b_R]
    Halving --> Cross[Cross-term Commitments L_k, R_k]
    Cross --> Challenge[Verifier Scalar Challenge x_k]
    Challenge --> Fold[Fold Vectors: a' = a_L * x_k + a_R * x_k^-1]
    Fold --> Check{Vector Length == 1?}
    Check -->|No| Halving
    Check -->|Yes| Base[Send Final Scalars a_0, b_0 with O(log N) Elements]
```

---

## 1. Key Properties & Complexity Metrics

| Property | Value | Description |
|---|---|---|
| **Proof Size** | $2 \lceil \log_2 n ceil + 4$ group elements | For 64-bit range proof: $pprox 672$ bytes. |
| **Setup Requirement** | Transparent | Uses discrete-log generators with NUMS (Nothing Up My Sleeve). |
| **Prover Time** | $O(n)$ | Multi-exponentiations on elliptic curve. |
| **Verifier Time** | $O(n)$ batched | Linear verification, but batchable into single MSM of size $2n$. |
| **Range Constraint** | $v \in [0, 2^n - 1]$ | Enforced via bit-decomposition constraints $a_L \cdot a_R = 0$. |

---

## 2. Vector Halving Protocol Step

Given vectors $\mathbf{a}, \mathbf{b} \in \mathbb{F}^n$ and challenge $x$:
$$\mathbf{a}' = x \mathbf{a}_{lo} + x^{-1} \mathbf{a}_{hi}$$
$$\mathbf{b}' = x^{-1} \mathbf{b}_{lo} + x \mathbf{b}_{hi}$$
The inner product relation transforms predictably:
$$\langle \mathbf{a}', \mathbf{b}' angle = \langle \mathbf{a}_{lo}, \mathbf{b}_{hi} angle x^2 + \langle \mathbf{a}, \mathbf{b} angle + \langle \mathbf{a}_{hi}, \mathbf{b}_{lo} angle x^{-2}$$
Cross-terms $L = g^{\mathbf{a}_{lo}} h^{\mathbf{b}_{hi}} u^{\langle \mathbf{a}_{lo}, \mathbf{b}_{hi} angle}$ and $R = g^{\mathbf{a}_{hi}} h^{\mathbf{b}_{lo}} u^{\langle \mathbf{a}_{hi}, \mathbf{b}_{lo} angle}$ are sent to verifier each round.
