# Rust Internals: Non-Lexical Lifetimes (NLL) Borrow Checker

Early Rust enforced lexical lifetimes tied to lexical code blocks `{}`. Non-Lexical Lifetimes (NLL) and the Polonius borrow checker track origins and liveness on the Control Flow Graph (CFG).

```mermaid
flowchart TD
    Source[Rust Source Code] --> MIR[Mid-Level Intermediate Representation MIR]
    MIR --> CFG[Control Flow Graph Points: P]
    CFG --> Polonius[Polonius / NLL Analysis]
    Polonius --> Rule{Does live borrow point intersect invalidating write?}
    Rule -->|No| Compile[Compiles Cleanly: Lifetime ends at last read!]
    Rule -->|Yes| BorrowError[Compile Error: Cannot borrow as mutable!]
```

---

## 1. Ergonomic Impact
Allows mutable borrows to end immediately after their last operational read or write statement without requiring unnatural nested braces.
