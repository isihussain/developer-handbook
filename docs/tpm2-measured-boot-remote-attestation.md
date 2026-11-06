# TPM 2.0 Measured Boot & Remote Attestation

Trusted Platform Modules (TPM 2.0) establish a hardware-rooted chain of trust by recording cryptographic hash measurements into Platform Configuration Registers (PCRs).

```mermaid
sequenceDiagram
    participant Firmware as UEFI / Coreboot
    participant Bootloader as GRUB / systemd-boot
    participant Kernel as Linux Kernel
    participant TPM as TPM 2.0 PCRs
    participant Verifier as Remote Attestation Server
    Firmware->>TPM: Extend PCR[0]: UEFI Firmware Hash
    Firmware->>Bootloader: Handover
    Bootloader->>TPM: Extend PCR[4]: Bootloader Hash
    Bootloader->>Kernel: Handover
    Kernel->>TPM: Extend PCR[9]: Kernel Image + Initrd + Cmdline
    Verifier->>TPM: Send Nonce Challenge
    TPM->>Verifier: TPM2_Quote(PCRs, Nonce, AK Signature)
    Note over Verifier: Verifies Quote signature using enrolled AK Certificate
```

---

## 1. PCR Extend Operation Invariant

PCRs cannot be written directly to arbitrary values; they can only be **extended**:

$$	ext{PCR}_{	ext{new}} = 	ext{Hash}\left(	ext{PCR}_{	ext{old}} \parallel 	ext{Measurement}ight)$$

This mathematical one-way accumulation ensures an attacker cannot erase evidence of booting an untrusted intermediate component without resetting the entire chip.
