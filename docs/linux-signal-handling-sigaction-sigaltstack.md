# Linux Internals: POSIX Signal Delivery and Alternate Signal Stacks

When a thread triggers a Stack Overflow (`SIGSEGV`), the kernel attempts to push the signal frame onto the exhausted stack, causing a double-fault and instant process termination.

```mermaid
flowchart TD
    StackOverflow[Thread exhausts call stack: Stack Overflow!] --> SignalTrap[Kernel generates SIGSEGV]
    SignalTrap --> CheckStack{Has sigaltstack been configured?}
    CheckStack -->|No| DoubleFault[Double Fault! Process killed silently!]
    CheckStack -->|Yes| AltStack[Pushes frame onto dedicated alternate emergency stack (SIGSTKSZ)]
    AltStack --> Handler[Application Signal Handler executes cleanly & logs stack trace!]
```

---

## 1. Best Practice (Rust / Go / C++)
Always register a dedicated signal stack with `sigaltstack()` during thread pool initialization to diagnose stack overflow crashes cleanly.
