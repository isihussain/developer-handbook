# Decentralized Web: IPFS CIDv1 Architecture

A Content Identifier (CID) is a self-describing cryptographic content address that incorporates codec, hashing algorithm, and digest length.

```mermaid
flowchart LR
    Data[Raw File Bytes] --> Hash[SHA2-256 / BLAKE3 Digest]
    Hash --> Multihash[Multihash: <code> <len> <digest>]
    Multihash --> Multicodec[Multicodec: e.g. 0x70 dag-pb]
    Multicodec --> CIDv1[Base32 Encoded CIDv1 String: bafybeic...]
```

---

## 1. Future-Proof Addressing
If SHA-256 is deprecated in the future, CIDs seamlessly transition to post-quantum hashes without breaking addressing contracts.
