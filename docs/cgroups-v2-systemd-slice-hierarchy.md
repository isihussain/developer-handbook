# Linux System Engineering: systemd Slice Hierarchy and Unified cgroups v2

systemd organizes all system processes into a unified resource tree using slices, scopes, and services under `/sys/fs/cgroup`.

```mermaid
flowchart TD
    Root["-.slice (Root cgroup)"] --> System["system.slice (OS Daemons: sshd, systemd-journald)"]
    Root --> User["user.slice (Logged-in User Sessions)"]
    Root --> Machine["machine.slice (Kubelet, Containerd & Virtual Machines)"]
    Machine --> KubeletSlice["kubelet.slice: Guaranteed node stability"]
    Machine --> PodSlice["kubepods.slice: BestEffort, Burstable, Guaranteed QoS Slices"]
```

---

## 1. OOM Protection via Hierarchy
Memory exhaustion in `kubepods.slice` triggers OOM termination inside container workloads without ever starving core system daemons in `system.slice`.
