# KVM: Hardware Virtualization (VMX) & Extended Page Tables (EPT)

Intel VMX (Virtual Machine Extensions) and AMD-V enable bare-metal virtualization by switching CPU contexts between root mode (host kernel) and non-root mode (guest VM).

```mermaid
sequenceDiagram
    participant Host as Host KVM (VMX Root Mode)
    participant CPU as Physical CPU Hardware
    participant Guest as Guest OS (VMX Non-Root Mode)
    Host->>CPU: VMPTRLD (Load VMCS)
    Host->>CPU: VMLAUNCH / VMRESUME
    CPU->>Guest: Transition to Non-Root Execution
    Note over Guest: Executes guest instructions via EPT translation
    Guest->>CPU: Privileged Action / EPT Misconfiguration
    CPU->>Host: VM-Exit (Saves Guest State to VMCS)
    Note over Host: Host emulates hardware action and issues VMRESUME
```

---

## 1. Extended Page Tables (EPT)

* **Two-Dimensional Walk:** Guest Virtual Addresses (GVA) translate to Guest Physical Addresses (GPA) via guest CR3, which then translate to Host Physical Addresses (HPA) via hardware EPT pointers.
* **VMCS Structure:** The Virtual Machine Control Structure records host state, guest state, VM-execution controls, and VM-exit reasons.
