# Ed25519 Digital Signature Scheme (RFC 8032)

Ed25519 provides public-key signatures over Twisted Edwards curve `Curve25519` with SHA-512.

---

## 1. Key Properties

- **Deterministic Signatures**: Nonce $r$ is derived cryptographically via $r = H(	ext{private\_prefix} \parallel M)$, eliminating security catastrophic failures from broken random number generators.
- **Side-Channel Immunity**: Curve operations avoid data-dependent memory lookups or branches.
- **Collision Resistance**: Full 128-bit security level with 32-byte public keys and 64-byte signatures.
