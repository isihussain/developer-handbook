# Linux seccomp User Notification Filters

`SECCOMP_RET_USER_NOTIF` allows an unprivileged process's system call to be intercepted and emulated by an unprivileged supervisor process.

```mermaid
sequenceDiagram
    autonumber
    Target->>Kernel: System Call (e.g. mount, mknod)
    Kernel->>Supervisor: Event Notification over seccomp fd
    Supervisor->>Target: Inspect /proc/PID/mem
    Supervisor->>Kernel: Inject Response (SECCOMP_USER_NOTIF_FLAG_CONTINUE or return code)
    Kernel-->>Target: Syscall returns emulated result
```

---

## 1. Core Workflow

1. Target process installs BPF filter returning `SECCOMP_RET_USER_NOTIF` and obtains a listener file descriptor.
2. Supervisor receives notification struct containing syscall arguments and PID.
3. Supervisor performs action and issues `ioctl(SECCOMP_IOCTL_NOTIF_SEND)`.
