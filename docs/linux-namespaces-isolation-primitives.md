# Container Architecture: Linux Namespaces Isolation Primitives

Linux containers are not lightweight virtual machines; they are regular Linux processes running under specific kernel namespace boundaries.

```mermaid
flowchart TD
    Process[Container Process: PID 1 inside namespace] --> MNT[mnt: Isolated Filesystem Mounts]
    Process --> PID[pid: Independent Process Tree]
    Process --> NET[net: Dedicated IP, Route Table & Veth pair]
    Process --> IPC[ipc: Isolated System V IPC & POSIX Queues]
    Process --> UTS[uts: Custom Hostname & Domain]
    Process --> USER[user: Root (UID 0) inside -> Non-root outside]
    Process --> CGROUP[cgroup: Virtualized /sys/fs/cgroup View]
```

---

## 1. System Calls
- `clone(flags)`: Spawns new child process in designated namespaces.
- `unshare(flags)`: Disassociates calling process from current namespaces.
- `setns(fd, nstype)`: Attaches process to existing namespaces (used by `docker exec`).
