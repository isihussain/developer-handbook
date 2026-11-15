# Cryptography: Threshold BLS Signatures & Key Generation

Threshold BLS signatures combine Boneh-Lynn-Shacham pairing-friendly elliptic curves with $(t, n)$ Shamir Secret Sharing, allowing any $t$ of $n$ distributed participants to construct a valid digital signature.

```mermaid
flowchart TD
    Dealer[Key Distribution: Shamir Polynomial f(x)] --> S1[Node 1: Share s1]
    Dealer --> S2[Node 2: Share s2]
    Dealer --> S3[Node 3: Share s3]
    S1 -->|Sign(m)| Sig1[Partial Signature sigma_1]
    S2 -->|Sign(m)| Sig2[Partial Signature sigma_2]
    S3 -->|Sign(m)| Sig3[Partial Signature sigma_3]
    Sig1 & Sig2 --> Aggregator[Lagrange Interpolation Aggregator]
    Aggregator --> CombinedSig[Threshold Signature: sigma = prod(sigma_i ^ lambda_i)]
```

---

## 1. Mathematical Structure (BLS12-381)

* **Pairing Map:** $e: \mathbb{G}_1 	imes \mathbb{G}_2 	o \mathbb{G}_T$.
* **Signing:** Each node signs message hash $H(m) \in \mathbb{G}_1$ using its secret share $s_i \in \mathbb{F}_q$:

$$\sigma_i = s_i \cdot H(m)$$

* **Lagrange Aggregation:** Any $t$ partial signatures combine into the full signature $\sigma$ using Lagrange basis polynomials:

$$\sigma = \sum_{i=1}^{t} \lambda_i \cdot \sigma_i \quad 	ext{where} \quad \lambda_i = \prod_{j 
eq i} rac{-x_j}{x_i - x_j}$$

* **Verification:** The aggregator verifies the signature against global public key $PK \in \mathbb{G}_2$:

$$e(\sigma, G_2) = e(H(m), PK)$$
