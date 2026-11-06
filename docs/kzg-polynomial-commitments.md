# KZG (Kate-Zaverucha-Goldberg) Polynomial Commitments

KZG commitments enable a prover to commit to a polynomial of arbitrary degree $d$ with a single elliptic curve group element and prove evaluations at any point with a single opening proof.

```mermaid
flowchart TD
    Setup[Structured Reference String: g1^s^0, g1^s^1, ..., g1^s^d, g2^s] --> Commit[Commitment C = P(s)*g1 = sum c_i * (g1^s^i)]
    Commit --> Open[Opening at z: Compute Quotient Q(x) = (P(x) - P(z)) / (x - z)]
    Open --> Proof[Proof Pi = Q(s)*g1]
    Proof --> Verify[Verifier Pairing Check: e(C - P(z)*g1, g2) == e(Pi, g2^s - z*g2)]
```

---

## 1. Mathematical Mechanics

### Step 1: Trusted Setup Ceremony (SRS)
Generates secret trapdoor $	au \in \mathbb{F}_p$ which is discarded:
$$	ext{SRS} = \left( \{g_1^{	au^i}\}_{i=0}^d, \{g_2^{	au^j}\}_{j=0}^1 ight)$$

### Step 2: Commitment
To commit to $P(X) = \sum_{i=0}^n c_i X^i$:
$$C = [P(	au)]_1 = \sum_{i=0}^n c_i \cdot [g_1^{	au^i}] \in \mathbb{G}_1$$

### Step 3: Evaluation Proof
To prove $P(z) = y$, polynomial remainder theorem guarantees $X - z$ divides $P(X) - y$:
$$Q(X) = rac{P(X) - y}{X - z}$$
$$\pi = [Q(	au)]_1 = \sum_{i=0}^{n-1} q_i \cdot [g_1^{	au^i}] \in \mathbb{G}_1$$

### Step 4: Verification via Bilinear Pairing
Checks consistency without knowing $	au$:
$$e(C - [y]_1, [1]_2) = e(\pi, [	au - z]_2)$$
