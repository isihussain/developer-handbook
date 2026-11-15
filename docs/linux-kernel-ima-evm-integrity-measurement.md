# Linux Kernel: Integrity Measurement Architecture (IMA) & EVM

The Linux Integrity Measurement Architecture (IMA) and Extended Verification Module (EVM) authenticate file contents and POSIX extended attributes before binary execution, blocking unauthorized software modifications.

```mermaid
flowchart TD
    SysExec[sys_execve / sys_open] --> IMA_Hook[IMA Hook: ima_file_check]
    IMA_Hook --> CheckPolicy{IMA Policy Match?}
    CheckPolicy -->|Yes| Measure[Calculate SHA-256 of file data]
    Measure --> TPM_Extend[Extend Hash to TPM PCR 10]
    Measure --> Appraise{EVM Appraisal}
    Appraise -->|Verify security.ima & security.evm HMAC| AllowExec[Allow Execution]
    Appraise -->|Invalid Signature / Hash Mismatch| DenyExec[Block with EACCES / EPERM]
```

---

## 1. Subsystem Roles

* **IMA-Measurement:** Maintains an append-only measurement log in kernel memory and extends file digests into TPM PCR 10 for remote attestation.
* **IMA-Appraisal:** Validates the cryptographic signature or hash stored in `security.ima` extended attribute against trusted kernel keyrings (`.ima`).
* **EVM (Extended Verification Module):** Protects file metadata (inodes, UID/GID, modes, and `security.ima` itself) against offline tampering by signing attributes with a hardware-backed HMAC (`security.evm`).
