# Cryptographic Hashing: Argon2id Memory-Hard Password Derivation

Argon2 won the Password Hashing Competition (PHC). The hybrid `Argon2id` variant balances memory-hardness to defeat GPU/ASIC crackers while resisting side-channel timing attacks.

```mermaid
flowchart TD
    Password[User Password + Salt] --> Argon2id[Argon2id Engine]
    Argon2id --> Pass1[First Pass: Argon2d Data-Independent (Side-channel safe)]
    Pass1 --> Pass2[Subsequent Passes: Argon2i Data-Dependent (ASIC memory-hard)]
    Pass2 --> LargeRAM[Fills 64MB - 1GB RAM Matrix: Prohibits cheap GPU parallel cracking]
    LargeRAM --> Hash[Final 256-bit Derived Key / Hash]
```

---

## 1. Recommended Production Parameters (OWASP)
- **Memory (m):** 65536 KiB (64 MiB)
- **Iterations (t):** 3 passes
- **Parallelism (p):** 4 threads
