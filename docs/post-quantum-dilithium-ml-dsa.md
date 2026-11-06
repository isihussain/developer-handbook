# NIST FIPS 204 ML-DSA (Dilithium) Lattice Signatures

ML-DSA is the standardized post-quantum digital signature algorithm based on Module Learning With Errors (M-LWE) and Module Short Integer Solution (M-SIS) problems using Fiat-Shamir with Aborts.

```mermaid
flowchart TD
    Sign[Signer: Sample masking vector y] --> Matrix[Compute w = A * y]
    Matrix --> HighBits[Extract High Bits w1]
    HighBits --> Challenge[Challenge c = H_hash m, w1]
    Challenge --> Candidate[Compute candidate signature z = y + c * s1]
    Candidate --> CheckNorm{Is ||z||_inf < gamma1 - beta?}
    CheckNorm -->|No: Abort & Retry| Sign
    CheckNorm -->|Yes: Acceptable| Hint[Compute decomposition hints h]
    Hint --> Output[Signature sigma = z, h, c]
```

---

## 1. FIPS 204 Parameter Sets

| Parameter | ML-DSA-44 | ML-DSA-65 | ML-DSA-87 |
|---|---|---|---|
| **NIST Security Level** | Level 2 (SHA-256) | Level 3 (AES-192) | Level 5 (AES-256) |
| **Matrix Size ($k 	imes l$)** | $4 	imes 4$ | $6 	imes 5$ | $8 	imes 7$ |
| **Public Key Size** | 1,312 bytes | 1,952 bytes | 2,592 bytes |
| **Signature Size** | 2,420 bytes | 3,309 bytes | 4,627 bytes |
| **Ring Modulus ($q$)** | $8380417 = 2^{23} - 2^{13} + 1$ | $8380417$ | $8380417$ |

---

## 2. Fiat-Shamir with Aborts

To prevent signatures from leaking the secret key $\mathbf{s}_1$ through statistical correlation:
1. Candidate signature is $\mathbf{z} = \mathbf{y} + c \cdot \mathbf{s}_1$.
2. If $\|\mathbf{z}\|_\infty \ge \gamma_1 - eta$, the signer **aborts** and restarts with fresh $\mathbf{y}$.
3. This rejection sampling enforces that the distribution of $\mathbf{z}$ is independent of $\mathbf{s}_1$, guaranteeing zero-knowledge signature output.
