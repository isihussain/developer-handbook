# Zero-Knowledge: Bulletproofs Range Proofs & Inner Product Arguments

Bulletproofs provide short, non-interactive zero-knowledge range proofs without requiring a trusted setup. They prove that a committed secret value lies within $[0, 2^n - 1]$ with $O(\log n)$ communication complexity.

```mermaid
flowchart LR
    Secret[Secret Value v in [0, 2^n-1]] --> Commitment[Pedersen Commitment: V = v*G + gamma*H]
    Commitment --> Vectors[Vector Polynomials: l(X) and r(X)]
    Vectors --> InnerProduct[Inner Product Argument: Recursive Reduction]
    InnerProduct --> Proof["Proof of size 2 * log2(n) group elements (e.g. 672 bytes)"]
```

---

## 1. Protocol Foundations

* **No Trusted Setup:** Built over standard discrete logarithm elliptic curves (e.g., Secp256k1, Ristretto255); security holds under the Discrete Logarithm Assumption.
* **Vector Inner Product Reduction:** Halves vector sizes $a$ and $b$ at each recursive round, transmitting cross-terms $L_k$ and $R_k$:

$$P_{k+1} = L_k^{u_k^2} \cdot P_k \cdot R_k^{u_k^{-2}}$$

* **Batch Verification:** Multiple range proofs can be aggregated and verified simultaneously using multi-scalar multiplication (MSM) algorithms (Pippenger).
