# Cryptography: Fully Homomorphic Encryption (FHE: BGV & CKKS)

Fully Homomorphic Encryption allows computational evaluation over encrypted ciphertexts without decrypting data, guaranteeing zero-trust privacy in cloud processing environments.

```mermaid
flowchart TD
    Plaintext[Client: Plaintext Data m] --> Encrypt[Encrypted under RLWE: ct = (c0, c1)]
    Encrypt --> CloudCompute[Untrusted Cloud Server]
    subgraph HomomorphicOps[Homomorphic Evaluation in Ring R_q]
        CloudCompute --> Add[Homomorphic Addition: ct1 + ct2]
        CloudCompute --> Mult[Homomorphic Multiplication: ct1 * ct2]
        Mult --> Relinearization[Relinearization: Reduces ciphertext size]
        Relinearization --> ModSwitch[Modulus Switching / Rescaling: Dampens noise growth]
    end
    ModSwitch --> Result[Encrypted Result ct_res]
    Result --> Decrypt[Client: Decrypts with Private Key]
```

---

## 1. Scheme Comparison

| Feature | BGV / BFV | CKKS |
|---|---|---|
| **Plaintext Space** | Exact modular integers ($\mathbb{Z}_t$) | Approximate floating-point numbers ($\mathbb{C}^N$) |
| **Primary Use-Case** | Exact database queries, financial balance logic | Encrypted ML inference, neural networks, statistics |
| **Noise Management** | Modulus Switching reduces noise variance | Rescaling acts as fixed-point rounding of ciphertexts |
| **Underlying Problem** | Ring Learning With Errors (RLWE) | Ring Learning With Errors (RLWE) |
