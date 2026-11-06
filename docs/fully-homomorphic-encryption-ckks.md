# CKKS & BFV Fully Homomorphic Encryption (FHE)

Homomorphic encryption allows arbitrary mathematical computations directly on ciphertexts without decryption, preserving confidentiality in untrusted cloud processing environments.

```mermaid
flowchart LR
    Plain[Plaintext Vectors / Integers] --> Encode[Encoding: Canonical Embedding / CRT]
    Encode --> Encrypt[RLWE Encryption: c = (c0, c1)]
    Encrypt --> Eval[Homomorphic Operations: Add, Mult, Relinearize, Rescale]
    Eval --> Decrypt[Decryption: Secret Key Inner Product]
    Decrypt --> Decode[Decoded Result]
```

---

## 1. CKKS vs BFV Schemes

| Feature | BFV (Brakerski-Fan-Vercauteren) | CKKS (Cheon-Kim-Kim-Song) |
|---|---|---|
| **Plaintext Domain** | Exact Integers $\mathbb{Z}_t$. | Approximate Complex Numbers $\mathbb{C}^{N/2}$ / Floats $\mathbb{R}$. |
| **Arithmetic Type** | Exact modular arithmetic. | Floating-point machine learning inference. |
| **Noise Management** | Noise grows multiplicatively; ModSwitching. | Rescaling reduces modulus and noise simultaneously. |
| **Primary Workloads** | Private database queries, exact integer tallying. | Neural network inference, genomic statistical analysis. |

---

## 2. CKKS Rescaling Mechanics

Multiplication of two ciphertexts encrypted at scale $\Delta$:
$$c_{mult} = c_1 \cdot c_2 \implies 	ext{Scale becomes } \Delta^2$$
To prevent exponential bit growth, the **Rescale** operation divides the ciphertext and modulus by $q_l$:
$$c' = \left\lfloor rac{c_{mult}}{q_l} ightceil \pmod{Q/q_l}$$
The new scale returns to $rac{\Delta^2}{q_l} pprox \Delta$, discarding low-order noise while preserving significant floating-point digits.
