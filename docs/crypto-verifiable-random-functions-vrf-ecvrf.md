# Cryptography: Verifiable Random Functions (ECVRF RFC 9381)

A Verifiable Random Function (VRF) operates as a cryptographic public-key version of a keyed hash: given an input $x$ and private key $SK$, it generates a pseudorandom output $Y$ along with an unforgeable proof $\pi$ that any observer can verify against $PK$.

```mermaid
sequenceDiagram
    participant Prover as Prover (Holds Secret Key SK)
    participant Verifier as External Verifier (Holds PK)
    Note over Prover: VRF_HashToCurve(PK, alpha) -> Point H<br/>Gamma = x * H<br/>Generates Schnorr-like DLEQ Proof pi
    Prover->>Verifier: Send Output Beta + Proof pi
    Note over Verifier: VRF_Verify(PK, alpha, pi)<br/>Validates that Gamma and PK share the same discrete log
    Note over Verifier: Computes Beta = Hash(Gamma) -> Accepts if matches
```

---

## 1. Core Cryptographic Invariants

* **Uniqueness:** For any fixed public key $PK$ and input $lpha$, there exists strictly one valid pseudorandom output value $eta$.
* **Pseudorandomness:** Without knowledge of secret key $SK$, output $eta$ is computationally indistinguishable from uniform random noise to any observer.
* **Consensus Applications:** Used in proof-of-stake distributed leader elections (e.g., Algorand, Cardano) to privately evaluate leadership lotteries without exposing identity before block proposal.
