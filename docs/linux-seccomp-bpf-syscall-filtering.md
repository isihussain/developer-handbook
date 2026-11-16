# Linux Security: Seccomp-BPF Syscall Filtering Architecture

Secure Computing with BPF (seccomp-bpf) inspects system call numbers and arguments at entry, allowing containers to restrict kernel attack surfaces.

```mermaid
flowchart TD
    Syscall[User Space System Call: e.g. ptrace()] --> SeccompHook[Seccomp BPF Filter]
    SeccompHook -->|SECCOMP_RET_ALLOW| Kernel[Execute Syscall Normally]
    SeccompHook -->|SECCOMP_RET_KILL| Terminate[SIGSYS: Kill Process]
    SeccompHook -->|SECCOMP_RET_ERRNO| ReturnErr[Return -EPERM to Userspace]
    SeccompHook -->|SECCOMP_RET_LOG| Log[Log Audit Event and Allow]
```

---

## 1. Filter Composition
Docker and Kubernetes default seccomp profiles block over 40 dangerous syscalls including `ptrace`, `kexec_load`, and `unshare`.
