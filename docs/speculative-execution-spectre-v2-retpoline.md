# Speculative Execution: Spectre v2 & Retpoline Trampolines

Spectre v2 (CVE-2017-5715) exploits branch target injection in out-of-order CPUs. The Retpoline (return trampoline) software mitigation isolates indirect branches inside infinite speculation loops.

```mermaid
sequenceDiagram
    participant Caller
    participant Trampoline as Retpoline Trampoline
    participant Target as Indirect Target Address
    Caller->>Trampoline: call __x86_indirect_thunk_rax
    Note over Trampoline: Captures speculative execution into pause loop
    Trampoline->>Target: Overwrites return address with %rax and executes 'ret'
    Target-->>Caller: Clean Return
```

---

## 1. Assembly Construction

```nasm
__x86_indirect_thunk_rax:
    call    .speculate_trap
.speculate_loop:
    pause
    lfence
    jmp     .speculate_loop
.speculate_trap:
    mov     %rax, (%rsp)    ; Replace return address with destination
    ret                     ; Ret matches RSB, halts speculative BTB injection
```
