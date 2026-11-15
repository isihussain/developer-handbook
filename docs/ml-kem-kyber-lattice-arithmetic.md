# NIST FIPS 203: ML-KEM (Kyber) Lattice Arithmetic Architecture

ML-KEM is the standardized post-quantum Key Encapsulation Mechanism based on the hardness of the Module Learning With Errors (M-LWE) problem over polynomial rings $R_q = \mathbb{Z}_q[X]/(X^{256} + 1)$.

```mermaid
flowchart TD
    Setup[Parameters: n=256, q=3329, eta] --> PolyRing[Polynomial Ring: Z_3329 / X^256 + 1]
    PolyRing --> NTT[Number Theoretic Transform: Fast O n log n Multiplication]
    NTT --> Matrix[Public Matrix A in R_q^{k x k} from SHAKE-128]
    Matrix --> LWE[Compute s and e small error vectors]
    LWE --> Public[Public Key: t = A * s + e]
    Public --> Encaps[Encapsulation: Ciphertext c = u, v + Shared Secret K]
```

---

## 1. Security Parameter Sets

| Parameter Set | Module Rank $k$ | Claimed NIST Security Level | Symmetric Equivalent |
|---|---|---|---|
| **ML-KEM-512** | 2 | Level 1 | AES-128 |
| **ML-KEM-768** | 3 | Level 3 | AES-192 |
| **ML-KEM-1024** | 4 | Level 5 | AES-256 |
