# Chaumian Blind Signatures & Ecash Mint Architecture

David Chaum's blind signature protocol allows users to obtain cryptographic token signatures from a central mint without revealing the serial numbers, providing cryptographic unlinkability.

```mermaid
sequenceDiagram
    participant User
    participant Mint as Ecash Mint Authority

    User->>User: Generate random token serial number x
    User->>User: Blind serial number: x_blind = x * r^e mod N
    User->>Mint: Send x_blind + Payment (Deposit)
    Mint->>Mint: Sign blinded token: s_blind = (x_blind)^d mod N
    Mint-->>User: Return s_blind
    User->>User: Unblind signature: s = s_blind * r^-1 mod N
    Note over User: User holds valid pair (x, s) where s^e = x mod N
    User->>Mint: Spend token (x, s) anonymously
    Mint->>Mint: Check x in Spent Database. If fresh: accept and record x!
```

---

## 1. Anonymity Guarantee

The mint cannot correlate the blinded token $x \cdot r^e \pmod N$ observed during minting with the unblinded token $x$ submitted during spending.
