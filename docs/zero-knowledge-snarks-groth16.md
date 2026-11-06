# Groth16 Zero-Knowledge SNARK Proof System

Groth16 (2016) is a pairing-based zk-SNARK offering minimal proof size and rapid verification time.

---

## 1. Efficiency Characteristics

- **Proof Size**: Exactly 3 group elements ($A \in \mathbb{G}_1, B \in \mathbb{G}_2, C \in \mathbb{G}_1$), amounting to 128 bytes on BN254 curves.
- **Verification Cost**: Fixed cost involving three cryptographic bilinear pairings:
  $$e(A, B) = e(lpha, eta) \cdot e(x, \gamma) \cdot e(C, \delta)$$
- **Requirement**: Requires a trusted setup ceremony for each specific arithmetic circuit.
