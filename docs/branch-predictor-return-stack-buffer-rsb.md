# CPU Internals: Return Stack Buffer (RSB) Speculation

Modern CPUs predict the target of `ret` instructions using a dedicated Return Stack Buffer (RSB). When function call depth exceeds RSB capacity, RSB underflow occurs.

```mermaid
sequenceDiagram
    participant CPU as CPU Instruction Pipeline
    participant RSB as Return Stack Buffer (16-32 Entries)
    participant Stack as Process Memory Call Stack

    Note over CPU: call function_A()
    CPU->>RSB: Pushes return address to RSB
    Note over CPU: call function_B() ... (Nested calls > 32)
    Note over RSB: RSB Underflow occurs!
    Note over CPU: CPU falls back to indirect branch predictor!
    Note over CPU: High misprediction risk & Spectre-RSB vulnerability!
```

---

## 1. Post-Spectre Padding
Operating systems and hypervisors pad the RSB with 32 benign return entries during context switches to prevent cross-process branch poisoning.
