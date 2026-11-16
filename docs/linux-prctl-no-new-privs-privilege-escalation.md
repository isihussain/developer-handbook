# Linux Security: PR_SET_NO_NEW_PRIVS and SUID Disabling

The `PR_SET_NO_NEW_PRIVS` flag ensures that a process and its children can never acquire additional privileges, even when executing `setuid` binaries.

```mermaid
sequenceDiagram
    participant Container as Container Process (Unprivileged UID 1000)
    participant Kernel as Linux Kernel

    Container->>Kernel: prctl(PR_SET_NO_NEW_PRIVS, 1, 0, 0, 0)
    Note over Kernel: Flag is immutable! Cannot be unset even by root!
    Container->>Kernel: execve('/bin/sudo') or SUID binary
    Note over Kernel: SUID bits and capabilities are completely IGNORED!
    Note over Kernel: Process runs strictly with UID 1000: Privilege Escalation Neutralized!
```

---

## 1. Container Engine Default
Docker and Podman apply this flag before executing container entrypoints to prevent container breakouts via misconfigured SUID binaries on mounted volumes.
