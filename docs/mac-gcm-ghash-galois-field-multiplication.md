# Cryptography: AES-GCM & GHASH Galois Field Multiplication

Galois/Counter Mode (GCM) combines counter-mode symmetric encryption with universal hash authentication computed over the finite field $GF(2^{128})$ defined by the irreducible polynomial $P(x) = x^{128} + x^7 + x^2 + x + 1$.

```mermaid
flowchart LR
    subgraph GHASH[GHASH Authenticator Pipeline]
        A0[AAD / Ciphertext Block X_i] --> XOR[XOR with Previous Hash Y_{i-1}]
        XOR --> GF_Mult["Multiplication over GF(2^128) by Hash Key H"]
        GF_Mult --> Y_i[Accumulated State Y_i]
    end
    Y_i --> Final[Encrypted with J0 -> Authentication Tag T]
```

---

## 1. Hardware Acceleration (PCLMULQDQ)

* **Carry-Less Multiplication:** Normal binary multiplication propagates carry bits; Galois field polynomial multiplication executes carry-less operations (equivalent to XOR).
* **Intel / ARM Instructions:**
  - x86: `PCLMULQDQ` computes carry-less multiplication of 64-bit quadwords into 128-bit results.
  - ARM: `PMULL` / `PMULL2` performs 64-bit polynomial multiplication in NEON registers.
* **Reduction Algorithm:** Karatsuba multiplication combined with Barrett reduction maps the 256-bit product back into 128 bits in constant time.
