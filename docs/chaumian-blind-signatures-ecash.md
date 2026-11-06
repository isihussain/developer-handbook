# Chaumian Blind Signatures & Cryptographic e-Cash

David Chaum's blind signature protocol allows a user to obtain a signature from an issuer without disclosing the message content, creating untraceable digital bearer tokens.

```mermaid
sequenceDiagram
    participant User
    participant Mint as Mint / Issuer
    Note over User: Generates secret token message m & blinding factor r
    User->>User: Blind message: m' = m * r^e mod N
    User->>Mint: Send blinded m' + deposit
    Note over Mint: Sign blinded message without seeing m
    Mint->>User: Blind signature s' = (m')^d mod N
    Note over User: Unblind: s = s' * r^-1 mod N
    Note over User: Token (m, s) is valid: s^e == m mod N
    User->>Mint: Redeem (m, s) anonymously
    Note over Mint: Verifies s^e == m, records m in spent list
```

---

## 1. RSA Blind Signature Protocol

* **Key Setup:** RSA public key $(e, N)$, private key $d$.
* **Blinding:** User selects random $r \in \mathbb{Z}_N^*$:
  $$m' = m \cdot r^e \pmod N$$
* **Signing:** Mint signs blinded message:
  $$s' = (m')^d = (m \cdot r^e)^d = m^d \cdot r^{e \cdot d} = m^d \cdot r \pmod N$$
* **Unblinding:** User removes blinding factor:
  $$s = s' \cdot r^{-1} = m^d \cdot r \cdot r^{-1} = m^d \pmod N$$
* **Verification:** Anyone verifies $s^e \equiv m \pmod N$. Mint cannot link $(m, s)$ to the withdrawal session where $m'$ was signed.
