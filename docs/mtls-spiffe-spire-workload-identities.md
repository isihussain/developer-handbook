# Zero-Trust Infrastructure: SPIFFE ID and SPIRE Attestation

The Secure Production Identity Framework for Everyone (SPIFFE) assigns cryptographic identities to microservices, eliminating static API tokens and shared secrets.

```mermaid
flowchart TD
    Workload[Microservice Workload Pod] --> Agent[SPIRE Workload Agent]
    Agent --> Attest[Node & Workload Attestation: Checks cgroups, k8s pod UID]
    Agent --> Server[SPIRE Server CA]
    Server --> SVID[X.509 SVID Certificate: spiffe://cluster.local/ns/prod/sa/order-service]
    SVID --> Workload
    Workload -->|mTLS Handshake with SVID| Peer[Peer Workload: Mutual Authentication & Encryption]
```

---

## 1. SVID Rotation
SPIFFE Verifiable Identity Documents (SVID) typically have short lifespans (e.g. 1 hour) and are automatically rotated via local UNIX domain sockets without restarting application pods.
