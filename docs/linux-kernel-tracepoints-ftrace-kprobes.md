# Linux Kernel: Dynamic Instrumentation with Ftrace & Kprobes

Linux provides low-overhead kernel tracing through static tracepoints, dynamic function tracing (Ftrace), and arbitrary instruction patching (Kprobes).

```mermaid
flowchart TD
    subgraph Ftrace[Ftrace: Function Tracing via -pg NOPs]
        F_Entry[Function Entry: 5-byte NOP] -->|Enabled via debugfs| F_Patch[Patched to call __fentry__ trampoline]
    end
    subgraph Kprobes[Kprobes: Breakpoint Injection]
        K_Target[Target Instruction in Kernel Memory] -->|Patched with INT3 / BRK| K_Trap[Breakpoint Exception Handler]
        K_Trap --> K_Handler[Executes custom probe logic]
        K_Handler --> K_Step[Single-steps original instruction out-of-line]
    end
```

---

## 1. Instrumentation Types

1. **Static Tracepoints:** Built-in conditional branch hooks placed by kernel developers; zero runtime overhead when disabled.
2. **Ftrace:** Compilers insert 5-byte `NOP` sequences at function entries (`-fpatchable-function-entry`), patched dynamically with jumps to tracers.
3. **Kprobes / Kretprobes:** Replaces target instruction bytes with breakpoint opcodes to intercept arbitrary kernel execution locations.
