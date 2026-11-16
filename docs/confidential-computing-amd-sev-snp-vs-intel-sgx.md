# Confidential Computing: AMD SEV-SNP vs Intel SGX Architecture

Hardware-enforced memory encryption protects execution state and data in-use from untrusted host hypervisors, cloud admins, and physical DRAM probing.

```mermaid
flowchart TD
    UntrustedCloud[Untrusted Cloud Hypervisor / Host OS] --> EnclaveBoundary{Memory Encryption Engine}
    EnclaveBoundary -->|Intel SGX| ProcessEnclave[Process-Level Enclaves: Encrypted PRM Memory Pages]
    EnclaveBoundary -->|AMD SEV-SNP| VMEnclave[Full VM Isolation: Reverse Map Table (RMP) prevents replay attacks]
```

---

## 1. Threat Model Differences
- **Intel SGX:** Enclave per application module; minimal Trusted Computing Base (TCB), but requires refactoring application binaries.
- **AMD SEV-SNP:** Encrypts entire unmodified guest virtual machine; protects against hypervisor memory tampering via hardware Secure Nested Paging.
