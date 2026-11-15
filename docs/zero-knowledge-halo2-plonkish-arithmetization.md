# Zero-Knowledge: Halo2 & Plonkish Arithmetization

Halo2 utilizes Plonkish arithmetization and polynomial commitment schemes without trusted setups (Inner Product Arguments / KZG), enabling custom gate composition and lookup tables.

```mermaid
flowchart TD
    subgraph PlonkishMatrix[2D Execution Trace Matrix]
        Cols["Columns: Fixed (Constants), Advice (Witness), Instance (Public)"]
        Rows["Rows: Execution Steps 0 .. 2^k - 1"]
    end
    PlonkishMatrix --> Gates[Custom Gates: High-degree polynomial constraints]
    PlonkishMatrix --> Lookups["Lookup Arguments (Plookup): XOR, Range Checks"]
    PlonkishMatrix --> Permutation[Permutation Argument: Copy Constraints]
    Gates & Lookups & Permutation --> VanishingPoly[Vanishing Polynomial Quotient t(X)]
```

---

## 1. Matrix Structural Anatomy

* **Advice Columns:** Private witness values supplied by the prover during execution.
* **Instance Columns:** Public inputs verified jointly by prover and verifier.
* **Fixed Columns:** Constants and selector polynomials defining gate behaviors.
* **Lookup Arguments:** Proves that an advice cell exists within a predefined lookup table (e.g., verifying 8-bit XOR operations without generating hundreds of algebraic multiplication gates).
