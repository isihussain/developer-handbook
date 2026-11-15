# PKI: Certificate Transparency (RFC 6962) & Merkle Audit Proofs

Certificate Transparency (CT) eliminates rogue and undetected Certificate Authority (CA) issuances through append-only, publicly auditable cryptographic Merkle Tree logs.

```mermaid
flowchart TD
    subgraph MerkleLog[Append-Only Merkle Tree Log]
        Root[Signed Tree Head: STH Hash] --> N1[Node Hash 0-1]
        Root --> N2[Node Hash 2-3]
        N1 --> L0[Leaf 0: Cert A]
        N1 --> L1[Leaf 1: Cert B]
        N2 --> L2[Leaf 2: Cert C]
        N2 --> L3[Leaf 3: Target Cert D]
    end
    Client[TLS Client: Browser] -->|Requires SCT in TLS Handshake| WebServer[Web Server]
    Client -->|Validates Inclusion Proof: Leaf 3, Node 2, Node 0-1| Root
```

---

## 1. Cryptographic Proofs

1. **Signed Certificate Timestamp (SCT):** Cryptographic promise by a log operator that a certificate has been queued for inclusion in the log.
2. **Inclusion Proof (Audit Proof):** Log returns an $O(\log N)$ sequence of sibling hashes proving a specific leaf exists in the tree root.
3. **Consistency Proof:** Proves that an updated tree of size $M$ extends an earlier validated tree of size $N$ without rewriting, reordering, or deleting prior leaves.
