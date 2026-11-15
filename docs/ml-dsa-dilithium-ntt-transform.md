# NIST FIPS 204: ML-DSA (Dilithium) Lattice Signature Architecture

ML-DSA is the standardized post-quantum digital signature algorithm based on the Module Learning With Errors and Module Short Integer Solution problems, utilizing rejection sampling to eliminate secret key leakage.

```mermaid
sequenceDiagram
    participant Signer
    participant Verifier

    Signer->>Signer: Sample masking vector y from polynomial ring
    Signer->>Signer: Compute w1 = HighBits(A * y)
    Signer->>Signer: Compute challenge c = H(M || w1)
    Signer->>Signer: Compute candidate signature z = y + c * s1
    Note over Signer: Rejection Sampling: If z or r0 exceeds bounds, discard and retry!
    Signer->>Verifier: Publish Signature (z, h) and Public Key
    Verifier->>Verifier: Check ||z||_infinity < gamma2 - beta and reconstruct w1
```

---

## 1. NTT Acceleration

Polynomial multiplications are accelerated by transforming polynomials to the frequency domain using the Number Theoretic Transform:
$$a \cdot b = 	ext{iNTT}(	ext{NTT}(a) \odot 	ext{NTT}(b))$$
reducing multiplication complexity from $O(n^2)$ to $O(n \log n)$.
