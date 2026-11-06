# Microarchitectural Vulnerabilities & Speculation Barriers

Spectre and Meltdown exploit out-of-order speculative CPU execution to leak secrets across security boundaries via cache timing side channels.

---

## 1. Hardware Barriers

- `lfence`: Speculation barrier on x86-64; prevents subsequent instructions from speculatively executing until all prior instructions retire.
- Retpoline: Return trampoline replacing indirect branch jumps with return instructions to neutralize Branch Target Injection (Spectre V2).
