# Platform Security: TPM 2.0 Measured Boot and PCR Architecture

Trusted Platform Module (TPM 2.0) measures cryptographic hashes of system firmware, bootloader, kernel, and initial ramdisk into Platform Configuration Registers (PCRs).

```mermaid
flowchart LR
    UEFI[UEFI Firmware] -->|Measure & Hash| PCR0[PCR 0: Core Root of Trust]
    PCR0 --> Bootloader[GRUB Bootloader]
    Bootloader -->|Measure & Hash| PCR4[PCR 4: Bootloader Hash]
    PCR4 --> Kernel[Linux Kernel + initramfs]
    Kernel -->|Measure & Hash| PCR9[PCR 9: Kernel Hash]
    subgraph RemoteAttestation
        TPM_Quote[TPM 2.0 Quote: Signs PCR state with Attestation Identity Key AIK]
        TPM_Quote --> Verifier[Remote Verifier: Confirms Node Has Not Been Rootkitted!]
    end
```

---

## 1. PCR Extend Operation
PCR registers cannot be overwritten arbitrarily; they can only be **extended**:
$$\text{PCR}_{new} = \text{SHA256}(\text{PCR}_{old} \parallel \text{Measurement})$$
Ensures an attacker cannot erase traces of malicious boot stages without resetting hardware.
