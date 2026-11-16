# Rust Memory Safety: std::pin::Pin and Self-Referential Types

Rust `async/await` compiles down to generator state machines that store references across yield points, creating self-referential structs that must never move in memory.

```mermaid
flowchart LR
    Future[Async Future State Machine] --> MoveAttempt[Attempt to move struct to new RAM address]
    MoveAttempt --> PinCheck{Is Type Wrapped in Pin<P>?}
    PinCheck -->|Pin<!Unpin>| Prohibit[PROHIBITED! Compiler prevents moving self-referential pointer!]
    PinCheck -->|Unpin| Allowed[Safe to move (Pointers not internal)]
```

---

## 1. Safety Invariant
Once a non-`Unpin` type is pinned, its memory address will remain constant until its `drop()` destructor runs.
