# FROST: Flexible Round-Optimized Schnorr Threshold Signatures

FROST provides $(t, n)$ threshold Schnorr signature generation in 2 network rounds, producing standard single-key Schnorr signatures indistinguishable from non-threshold signatures.

```mermaid
sequenceDiagram
    participant Coor as Coordinator / SA
    participant P1 as Participant 1
    participant P2 as Participant 2
    participant P3 as Participant 3
    Note over P1,P3: Round 1: Preprocessing (Nonce Commitment)
    P1->>Coor: Public nonces (D_1, E_1)
    P2->>Coor: Public nonces (D_2, E_2)
    P3->>Coor: Public nonces (D_3, E_3)
    Note over Coor: Selects t signers & constructs Commitment List B
    Coor->>P1: Message m, Signing group, Commitment list B
    Coor->>P2: Message m, Signing group, Commitment list B
    Note over P1,P2: Round 2: Signature Share Generation
    P1->>Coor: Signature share z_1 = d_1 + (e_1 * rho_1) + lambda_1 * s_1 * c
    P2->>Coor: Signature share z_2 = d_2 + (e_2 * rho_2) + lambda_2 * s_2 * c
    Note over Coor: Combines z = sum(z_i) mod q, outputs (R, z)
    Coor->>Coor: Verifies g^z == R * Y^c
```

---

## 1. Resistance to Dorking & Wagner Attacks

Earlier threshold Schnorr schemes suffered from sub-exponential forgery attacks (Wagner's generalized birthday problem) during concurrent signing sessions.

FROST prevents this through **binding factors** $ho_i$:
1. Each participant commits to two nonces: $(d_{i,1}, e_{i,1})$ yielding public commitments $(D_{i,1}, E_{i,1})$.
2. The coordinator aggregates all nonces into commitment list $B = \{(i, D_i, E_i)\}_{i \in S}$.
3. Each participant deterministically computes binding factor:
   $$ho_i = H_1(i, m, B)$$
4. The composite group commitment becomes:
   $$R = \sum_{i \in S} \left( D_i + ho_i E_i ight)$$
5. Binds each participant's nonce linearly to the exact set of co-signers and message, neutralizing concurrency attacks.
