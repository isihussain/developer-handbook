# KVM & Intel VMX Hardware-Assisted Virtualization

Intel VT-x (VMX) introduces root and non-root CPU operating modes. The kernel controls guests through the Virtual Machine Control Structure (VMCS), executing guest instructions natively until intercepted operations trigger a VM-Exit.

```mermaid
stateDiagram-v2
    VMX_Root_Kernel: VMX Root Mode (Host Kernel KVM)
    VMX_NonRoot_Guest: VMX Non-Root Mode (Guest VM Execution)

    [*] --> VMX_Root_Kernel: VMXON
    VMX_Root_Kernel --> VMX_NonRoot_Guest: VMLAUNCH / VMRESUME
    VMX_NonRoot_Guest --> VMX_Root_Kernel: VM-Exit (I/O, EPT Violation, CR3 write)
    VMX_Root_Kernel --> [*]: VMXOFF
```

---

## 1. VMCS Control Areas

* **Guest-State Area:** Processor register snapshots loaded on VM-Entry.
* **Host-State Area:** Kernel state restored automatically on VM-Exit.
* **VM-Execution Control Fields:** Determines which instructions trigger traps (e.g. `INVD`, `CR3` load, `MSR` access).
* **VM-Exit Information Fields:** Exposes exit reason, qualification register values, and fault addresses.
