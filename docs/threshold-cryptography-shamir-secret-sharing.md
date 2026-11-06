# Shamir's Secret Sharing & Threshold Signatures (FROST)

Splits cryptographic secrets into $n$ shares such that any $k$ shares reconstruct the secret, but $k-1$ shares reveal zero information.

---

## 1. Polynomial Formulation

$$f(x) = S + a_1 x + a_2 x^2 + \dots + a_{k-1} x^{k-1} \pmod p$$
Shares $(x_i, f(x_i))$ allow secret $S = f(0)$ reconstruction via Lagrange polynomial interpolation:
$$S = \sum_{j=1}^k y_j \prod_{m 
e j} rac{x_m}{x_m - x_j} \pmod p$$
