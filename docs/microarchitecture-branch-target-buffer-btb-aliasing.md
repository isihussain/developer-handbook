# CPU Microarchitecture: Branch Target Buffer (BTB) Aliasing & Branch Prediction

Branch Target Buffers cache target jump addresses using truncated program counter (PC) hashes, creating potential aliasing collisions where disjoint branch instructions share identical hardware BTB predictor entries.

```mermaid
flowchart LR
    Branch1[Kernel Syscall Entry: Address 0xFFFF...4A80] -->|Hashed PC Index: Bits 11:2| Slot34[BTB Cache Slot 34: Predicts Target Address]
    Branch2[User Application: Address 0x0000...4A80] -->|Identical Hash Index: Collision!| Slot34
    Slot34 --> Speculation[CPU Speculatively Jumps to Cached Address]
```

---

## 1. Security & Performance Impact

1. **Branch Target Injection (Spectre v2):** An unprivileged user program executes branch instructions engineered to map to the same BTB entry as a target kernel branch, steering kernel speculative execution toward disclosure gadgets.
2. **Hardware Mitigations:**
   - **IBRS (Indirect Branch Restricted Speculation):** Hardware MSR flag preventing user code branches from influencing supervisor indirect predictions.
   - **IBPB (Indirect Branch Prediction Barrier):** Commands the CPU to flush all BTB branch predictor states across context switches.
