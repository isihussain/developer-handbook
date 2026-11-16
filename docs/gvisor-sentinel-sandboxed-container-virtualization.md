# Security Virtualization: Google gVisor Architecture

gVisor provides defense-in-depth container isolation by intercepting and implementing Linux system calls in a memory-safe userspace kernel called **Sentry**.

```mermaid
flowchart TD
    App[Untrusted Container Application] --> Syscall[Application System Call: socket(), clone()]
    Syscall --> KVM[Platform Virtualization: KVM / ptrace]
    KVM --> Sentry[gVisor Sentry: 300+ Syscalls implemented in Go!]
    Sentry --> Gofer[gVisor Gofer: Mediates host filesystem access over 9P/virtio-fs]
    Sentry -.->|Only safe host syscalls| HostKernel[Linux Host Kernel: Completely Isolated from Untrusted Code!]
```

---

## 1. Attack Surface Reduction
Container code never executes raw kernel syscalls directly on the host, neutralizing host kernel local privilege escalation (LPE) exploits.
