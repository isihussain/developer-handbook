# Linux Kernel Live Patching Architecture (klp)

Enables security patching of running kernel code without requiring system reboots.

```mermaid
flowchart TD
    Call[Caller invokes vulnerable_function] --> Ftrace[ftrace mcount / fentry stub]
    Ftrace --> KLP{klp active?}
    KLP -->|Yes| New[Redirect to patched_function]
    KLP -->|No| Old[Execute original vulnerable_function]
```

---

## 1. Consistency Model

Linux livepatching utilizes a per-task consistency model:
- Threads running during patch transition complete their current syscall before switching to the new version.
- Sleeping tasks switch execution to patched functions upon waking.
