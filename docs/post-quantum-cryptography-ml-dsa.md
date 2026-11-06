# NIST FIPS 204 ML-DSA (Module-Lattice Digital Signatures)

ML-DSA (formerly CRYSTALS-Dilithium) provides quantum-resistant digital signatures based on the hardness of lattice problems over module rings.

---

## 1. Parameter Sets & Dimensions

| Parameter Set | Security Level | Public Key Size | Signature Size |
|---|---|---|---|
| **ML-DSA-44** | NIST Level 2 | 1,312 bytes | 2,420 bytes |
| **ML-DSA-65** | NIST Level 3 | 1,952 bytes | 3,309 bytes |
| **ML-DSA-87** | NIST Level 5 | 2,592 bytes | 4,627 bytes |

Employs Fiat-Shamir with aborts to prevent signing secrets from leaking via distribution skews.
