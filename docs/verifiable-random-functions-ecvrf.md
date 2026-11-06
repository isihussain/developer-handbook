# Verifiable Random Functions (VRF)

A Verifiable Random Function produces a deterministic pseudorandom output along with a cryptographic proof that the output was computed correctly using the owner's private key.

```mermaid
sequenceDiagram
    participant Prover
    participant Verifier
    Note over Prover: Secret key sk, public key pk = sk * G
    Prover->>Prover: Hash input to curve point: H = HashToCurve(alpha)
    Prover->>Prover: Gamma = sk * H
    Prover->>Prover: Nonce proof (c, s) via DLEQ Schnorr protocol
    Prover->>Verifier: VRF Output beta = HashToBytes(Gamma), Proof pi = (Gamma, c, s)
    Note over Verifier: Verifier confirms DLEQ equality: log_G(pk) == log_H(Gamma)
    Note over Verifier: Recomputes beta = HashToBytes(Gamma)
```

---

## 1. Core Cryptographic Invariants

Given input $lpha$, secret key $SK$, and public key $PK$:
1. **Uniqueness:** No adversary can find $(PK, lpha)$ and two proofs $\pi_1 
eq \pi_2$ yielding distinct valid outputs $eta_1 
eq eta_2$.
2. **Pseudorandomness:** Without the proof $\pi$, the output $eta = 	ext{VRF\_Hash}(SK, lpha)$ is indistinguishable from uniform random bytes to any polynomial-time observer.
