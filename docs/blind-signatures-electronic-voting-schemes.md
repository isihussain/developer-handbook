# Cryptographic Privacy: David Chaum Blind Signatures Protocol

Blind signatures allow an authority to sign a document without reading its content, functioning as the digital equivalent of a carbon-paper envelope.

```mermaid
sequenceDiagram
    participant Voter
    participant Authority as Election Commission (Signing Authority)
    participant BallotBox as Public Ballot Box

    Note over Voter: Vote: 'Alice'. Blinds vote with random factor r: m' = m * r^e mod N
    Voter->>Authority: Sends Blinded Vote m'
    Note over Authority: Validates voter eligibility and signs blinded vote: s' = (m')^d mod N
    Authority-->>Voter: Returns Blinded Signature s'
    Note over Voter: Unblinds signature: s = s' / r mod N
    Voter->>BallotBox: Submits (Vote: 'Alice', Signature: s) anonymously over Tor
    Note over BallotBox: Verifies signature with Authority's public key: s^e == m mod N!
```

---

## 1. Mathematical Guarantee
The Authority cannot link the unblinded vote in the ballot box back to the original voter because $r$ is uniform and private to the voter.
