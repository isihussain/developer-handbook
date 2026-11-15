# Executable Binaries: DWARF .eh_frame Zero-Cost Exception Handling

Zero-cost exception handling incurs zero CPU overhead during normal execution paths by encoding stack unwinding metadata into read-only DWARF `.eh_frame` tables evaluated only when an exception is thrown.

```mermaid
sequenceDiagram
    participant App as Application Code: throw Exception()
    participant Unwinder as System Unwinder: _Unwind_RaiseException
    participant FrameTable as DWARF .eh_frame & LSDA Tables
    participant Personality as Personality Routine: __gxx_personality_v0
    participant Handler as Target catch Block
    App->>Unwinder: Invokes throw
    Note over Unwinder: Phase 1: Search Phase (Finds matching catch block)
    Unwinder->>FrameTable: Decodes CIE & FDE records using current RIP
    FrameTable->>Personality: Evaluates LSDA landing pad actions
    Note over Personality: Matching Handler found!
    Note over Unwinder: Phase 2: Cleanup Phase (Executes destructors)
    Unwinder->>Handler: Jumps to Landing Pad RIP with registers restored
```

---

## 1. Data Structures

* **CIE (Common Information Entry):** Defines architecture calling conventions, code alignment factors, and return address register ID.
* **FDE (Frame Description Entry):** Encodes table transitions that map instruction address ranges (`PC`) to Canonical Frame Address (CFA) offset rules.
* **LSDA (Language Specific Data Area):** Encodes language-specific exception tables, type filters, and landing pad addresses.
