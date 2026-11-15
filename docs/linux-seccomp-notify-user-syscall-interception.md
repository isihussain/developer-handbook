# Linux: Seccomp User Notification (SECCOMP_RET_USER_NOTIF)

Seccomp user notifications allow an unprivileged container process to delegate blocked system calls to an external supervisor process over a UNIX domain socket.

```mermaid
sequenceDiagram
    participant Container as Container Worker
    participant Kernel as Linux Kernel Seccomp
    participant Supervisor as Host Supervisor Daemon
    Container->>Kernel: sys_mknod(/dev/null)
    Kernel->>Supervisor: Delivers seccomp_notif on notify_fd
    Note over Supervisor: Inspects syscall arguments in /proc/PID/mem
    Supervisor->>Kernel: Issues SECCOMP_IOCTL_NOTIF_RECV + NOTIF_SEND
    Kernel-->>Container: Syscall returns 0 (Emulated Successfully)
```

---

## 1. Capabilities & Pitfalls

* **Privileged Emulation:** Allows rootless containers to run operations like `mount()` or `mknod()` through host supervisor delegation.
* **TOCTOU Guard:** Kernel requires `SECCOMP_IOCTL_NOTIF_ID_VALID` checks to ensure the target process did not terminate or swap memory during inspection.
