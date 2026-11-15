# Post-Quantum: Isogeny-Based Cryptography & CSIDH

Isogeny-based cryptography constructs cryptographic key exchanges by traversing isogeny graphs between supersingular elliptic curves, relying on the hardness of finding paths between isogenous curves.

```mermaid
flowchart TD
    CurveE0[Starting Supersingular Curve E_0: y^2 = x^3 + x] --> ActionA[Alice: Secret Ideal Class [a]]
    CurveE0 --> ActionB[Bob: Secret Ideal Class [b]]
    ActionA --> CurveEA["Alice Public Curve E_A = [a] * E_0"]
    ActionB --> CurveEB["Bob Public Curve E_B = [b] * E_0"]
    CurveEA --> SharedSecret["Shared Curve E_AB = [a] * E_B = [b] * E_A"]
    CurveEB --> SharedSecret
```

---

## 1. Mathematical Mechanics

* **Group Action:** CSIDH (Commutative Supersingular Isogeny Diffie-Hellman) defines a group action of an ideal class group $	ext{cl}(\mathcal{O})$ on supersingular elliptic curves over $\mathbb{F}_p$.
* **Velu's Formulas:** Computes explicit polynomial mappings (isogenies $\phi: E 	o E/G$) given a finite subgroup $G$ of kernel points.
* **Quantum Resistance:** Resists Shor's algorithm because the underlying structure is non-abelian hidden shift rather than a standard hidden subgroup problem over cyclic groups.
