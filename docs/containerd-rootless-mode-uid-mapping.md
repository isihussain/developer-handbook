# Container Security: Rootless containerd and User Namespace UID Mapping

Rootless containers run both container engine and container processes without root privileges, neutralizing container breakout vulnerabilities.

```mermaid
flowchart LR
    HostUser[Host Unprivileged User: UID 1001] --> UserNS[Linux User Namespace Mapping]
    UserNS --> ContainerRoot[Container View: UID 0 (Fake Root)]
    ContainerRoot --> AttemptBreakout[Attacker Breaks Out of Container!]
    AttemptBreakout --> HostBoundary[Attacker Arrives on Host as UID 1001: Zero Root Access!]
```

---

## 1. Subuid / Subgid Configuration
- `/etc/subuid`: `developer:100000:65536`
- Maps inside container UID 0-65535 to outside UID 100000-165535 on the host kernel.
