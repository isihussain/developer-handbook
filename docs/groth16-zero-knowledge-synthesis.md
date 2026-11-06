# Groth16 zk-SNARK Protocol Synthesis

Groth16 achieves the smallest proof size (3 group elements) and fastest verification (3 pairings) among all zero-knowledge argument systems at the cost of a circuit-specific trusted setup.

```mermaid
flowchart TD
    R1CS[R1CS: A*s o B*s = C*s] --> QAP[Quadratic Arithmetic Program: A(x)*B(x) - C(x) = H(x)*T(x)]
    QAP --> Setup[Circuit-Specific Setup: alpha, beta, gamma, delta, x in SRS]
    Setup --> Prover[Prover: Constructs A in G1, B in G2, C in G1]
    Prover --> Verify[Verifier Pairing Check: e(A, B) == e(alpha, beta) * e(x, gamma) * e(C, delta)]
```

---

## 1. Proof Elements & Proof Size

Groth16 proof consists of exactly 3 curve elements:
* $A \in \mathbb{G}_1$ (e.g. 48 bytes on BLS12-381 / 32 bytes on BN254)
* $B \in \mathbb{G}_2$ (e.g. 96 bytes on BLS12-381 / 64 bytes on BN254)
* $C \in \mathbb{G}_1$ (e.g. 48 bytes on BLS12-381 / 32 bytes on BN254)
* **Total Proof Size on BN254:** $32 + 64 + 32 = 128	ext{ bytes}$.

---

## 2. Verification Equation

Given public inputs $(x_1, \dots, x_l)$, verifier checks:

$$e(A, B) = e(lpha, eta) \cdot e\left(\sum_{i=0}^l x_i \cdot rac{eta A_i(x) + lpha B_i(x) + C_i(x)}{\gamma}, \gammaight) \cdot e(C, \delta)$$

Precomputing $e(lpha, eta)$ reduces verification to 3 runtime pairings.
