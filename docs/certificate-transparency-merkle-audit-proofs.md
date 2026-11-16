# Web Security: Certificate Transparency (RFC 6962) Architecture

Certificate Transparency (CT) eliminates rogue or compromised Certificate Authorities (CAs) by mandating that every issued TLS certificate be logged in public append-only Merkle trees.

```mermaid
flowchart TD
    CA[Certificate Authority] --> Submit[Submit Pre-Certificate to CT Log]
    Submit --> SCT[Signed Certificate Timestamp (SCT)]
    SCT --> Embed[Embed SCT in TLS Certificate]
    Embed --> Browser[Browser Client: Verifies Merkle Inclusion Proof]
    Browser --> Monitor[Auditor / Domain Monitor: Detects Unauthorized Issuance!]
```

---

## 1. Invariants
- Modern browsers (Chrome, Safari, Firefox) immediately reject TLS certificates lacking valid SCT proofs from at least two independent CT logs.
