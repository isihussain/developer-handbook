# NIST FIPS 203 ML-KEM (Module-Lattice KEM)

ML-KEM (formerly CRYSTALS-Kyber) is the standardized post-quantum key encapsulation mechanism based on Module Learning-with-Errors (M-LWE).

---

## 1. Security Parameter Sets

| Parameter Set | Security Level | Public Key Size | Ciphertext Size |
|---|---|---|---|
| **ML-KEM-512** | NIST Level 1 (AES-128 equivalent) | 800 bytes | 768 bytes |
| **ML-KEM-768** | NIST Level 3 (AES-192 equivalent) | 1,184 bytes | 1,088 bytes |
| **ML-KEM-1024** | NIST Level 5 (AES-256 equivalent) | 1,568 bytes | 1,568 bytes |

Hybrid key exchange pairs ML-KEM-768 with classical X25519 to defend against harvest-now-decrypt-later attacks.
