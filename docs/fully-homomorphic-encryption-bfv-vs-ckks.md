# Encrypted Computation: Fully Homomorphic Encryption (FHE) Schemes

Homomorphic encryption allows mathematical operations on ciphertexts such that decrypting the result matches the computation on plaintexts: $\text{Dec}(\text{Enc}(a) \oplus \text{Enc}(b)) = a + b$.

```mermaid
flowchart TD
    Plaintext[Plaintext Data] --> Encrypt[Encrypt with RLWE Lattice Scheme]
    Encrypt --> Ciphertext[Ciphertext with Noise Budget]
    Ciphertext --> Choice{FHE Scheme}
    Choice -->|BFV / BGV| ExactInt[Exact Modular Integer Arithmetic: Cryptographic voting, private set intersection]
    Choice -->|CKKS| ApproxFloat[Approximate Floating-Point Arithmetic: Privacy-preserving AI inference & genomics]
    ApproxFloat --> Bootstrap[Bootstrapping: Resets noise budget before overflow!]
```

---

## 1. Noise Budget Management
Every homomorphic multiplication increases ciphertext noise. When noise exceeds the lattice bound, decryption fails. **Bootstrapping** homomorphically evaluates the decryption circuit to generate a clean ciphertext.
