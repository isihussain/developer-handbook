# Linux Containers: User Namespaces & UID/GID Mapping

User Namespaces (`CLONE_NEWUSER`) enable unprivileged users to operate as `root` (UID 0) inside container boundaries while remaining completely unprivileged on the host system.

```mermaid
flowchart LR
    subgraph Container[Container Boundary: Namespace]
        C_Root[Container Process: UID 0 (root)]
    end
    subgraph Host[Host System: Global Namespace]
        H_User[Host Process: UID 100000 (Unprivileged)]
    end
    C_Root -->|/proc/PID/uid_map translation: 0 -> 100000| H_User
```

---

## 1. Security & Configuration

* **`uid_map` Syntax:** `ID-inside-ns   ID-outside-ns   length` (e.g., `0 100000 65536`).
* **Privilege Containment:** Even if an attacker escapes container root isolation, they operate as an unprivileged high-number UID on the host, preventing host root filesystem modification.
