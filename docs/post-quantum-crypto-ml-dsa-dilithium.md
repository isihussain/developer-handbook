# Post-Quantum Security: NIST ML-DSA (CRYSTALS-Dilithium) Signatures

NIST ML-DSA provides post-quantum digital signature verification based on the hardness of the Module Learning With Errors (M-LWE) and Module Short Integer Solution (M-SIS) problems.

```mermaid
flowchart LR
    Message[Document / Message] --> FiatShamir[Fiat-Shamir with Aborts Protocol]
    FiatShamir --> RejectionSample{Reject if signature leaks private key norms?}
    RejectionSample -->|Yes| Retry[Regenerate with fresh masking vector]
    RejectionSample -->|No| Output[Valid Post-Quantum Signature]
```

---

## 1. Rejection Sampling Invariant
Signatures are sampled from a distribution that is statistically independent of the secret key, preventing differential power and lattice leakage attacks.
