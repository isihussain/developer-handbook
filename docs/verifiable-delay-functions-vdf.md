# Verifiable Delay Functions (VDF) Architecture

Functions that require a mandatory, non-parallelizable sequential time delay to compute, but can be verified exponentially faster.

---

## 1. Wesolowski VDF

- **Evaluation**: Computes $y = x^{2^T} \pmod N$ in an RSA group of unknown order. Requires $T$ sequential squaring operations.
- **Verification**: Generates a proof $\pi = x^{\lfloor 2^T / \ell floor} \pmod N$ verified in $O(\log T)$ time using a prime $\ell$.
