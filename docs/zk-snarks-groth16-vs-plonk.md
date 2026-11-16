# Zero-Knowledge Cryptography: Groth16 vs PLONK Architecture

zk-SNARKs allow a prover to convince a verifier that a statement is true without revealing witness data. Groth16 requires trusted setup per circuit, whereas PLONK uses universal setup.

```mermaid
flowchart TD
    Computation[Computational Problem] --> R1CS[R1CS / Plonkish Arithmetization]
    R1CS --> Choice{Proving Scheme}
    Choice -->|Groth16| SetupG[Circuit-Specific Trusted Setup: Toxic Waste per app!]
    SetupG --> ProofG[Fastest Proof: 3 Group Elements (128 bytes) & 3ms verify]
    Choice -->|PLONK| SetupP[Universal SRS (KZG Commitment Ceremony)]
    SetupP --> ProofP[Universal Setup: Supports dynamic gate polynomials & permutation arguments]
```

---

## 1. Comparison Matrix

| Property | Groth16 | PLONK |
| :--- | :--- | :--- |
| **Trusted Setup** | Per-circuit (Inflexible) | Universal & Updatable (KZG) |
| **Proof Size** | Minimal (~128 bytes) | Small (~400-800 bytes) |
| **Verification Time** | ~1-3 ms (3 pairings) | ~3-6 ms |
| **Arithmetization** | R1CS (Rank-1 Constraint System) | Plonkish (Custom gates + Permutation) |
