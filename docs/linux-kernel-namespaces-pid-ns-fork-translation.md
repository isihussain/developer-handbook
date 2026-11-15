# Linux Containers: PID Namespaces & Process Hierarchies

PID Namespaces (`CLONE_NEWPID`) provide isolated process ID virtualizations: a single process has distinct numeric PIDs across different nested namespace layers, anchoring PID 1 inside container boundaries.

```mermaid
flowchart TD
    subgraph HostNS[Host Initial PID Namespace: Level 0]
        HostInit[Host systemd: PID 1]
        ContainerFork[Container Supervisor: PID 1042]
        WorkerHost[Container Worker: PID 1043]
    end
    subgraph ContainerNS[Container Child Namespace: Level 1]
        ContainerInit[Container Init: PID 1 (Inside Container)]
        WorkerContainer[Child Worker: PID 2 (Inside Container)]
    end
    ContainerFork -.->|Translates to| ContainerInit
    WorkerHost -.->|Translates to| WorkerContainer
```

---

## 1. Kernel Representation & PID 1 Semantics

* **`struct pid` Data Structure:**
  ```c
  struct pid {
      refcount_t count;
      unsigned int level;
      struct upid numbers[1]; /* Flexible array: one upid per active level */
  };
  ```
* **PID 1 Signal Handling:** Inside a PID namespace, signals (like `SIGTERM`, `SIGINT`) sent to PID 1 are discarded by default unless the init process explicitly registers a signal handler.
* **Namespace Termination:** If PID 1 inside a namespace terminates, the Linux kernel terminates all remaining descendant processes in that namespace with `SIGKILL`.
