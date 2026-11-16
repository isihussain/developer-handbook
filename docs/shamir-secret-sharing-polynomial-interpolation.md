# Cryptographic Key Management: Shamir Secret Sharing (k of n)

Shamir's Secret Sharing splits a secret $S$ into $n$ distinct shares such that any $k$ shares can reconstruct $S$, but $k-1$ shares reveal zero mathematical information.

```mermaid
flowchart TD
    Secret["Secret S = f(0)"] --> Poly["Random Polynomial: f(x) = S + a1*x + a2*x^2 + ... + a_{k-1}*x^{k-1} mod p"]
    Poly --> Share1["Share 1: (1, f(1))"]
    Poly --> Share2["Share 2: (2, f(2))"]
    Poly --> ShareN["Share n: (n, f(n))"]
    subgraph Reconstruction
        Share1 --> Lagrange[Lagrange Polynomial Interpolation at x=0]
        Share2 --> Lagrange
        Lagrange --> SecretRecovered["Exact Secret S Recovered!"]
    end
```

---

## 1. Information-Theoretic Security
Even with infinite computing power, having $k-1$ points leaves every possible value of $S$ in finite field $\mathbb{F}_p$ equally probable.
