# Runtime Systems: Stackful Fibers vs Stackless Coroutines

Concurrency runtimes organize cooperative multitasking using either stackful coroutines (independent dynamic stacks) or stackless coroutines (compiler-generated state machines).

```mermaid
flowchart TD
    subgraph Stackful[Stackful Coroutines: Go Goroutines, Boost.Fiber]
        StackMem[Dedicated Call Stack: e.g. 2KB-8KB dynamic stack]
        StackMem --> DeepCall[Can yield inside deeply nested child function frames]
        DeepCall --> CtxSwitch[CPU Register Context Switch: RSP, RIP swap]
    end
    subgraph Stackless[Stackless Coroutines: Rust async, C++20 co_await]
        HeapFrame[Compiler-Generated Frame Object on Heap]
        HeapFrame --> StateMachine[Transforms function into State Machine enum]
        StateMachine --> YieldPoint[Can ONLY yield at explicit await suspension points]
    end
```

---

## 1. Architectural Comparison

| Attribute | Stackful Coroutines | Stackless Coroutines |
|---|---|---|
| **Memory Overhead** | Higher (2KB to 8KB stack per fiber) | Minimal (Bytes needed for live variables) |
| **Yield Capability** | Arbitrary nested depth within call stack | Only at designated suspension points (`await`) |
| **FFI Compatibility** | Direct invocation of standard C functions | Requires bridging or offloading to thread pools |
| **Context Switch** | Saves and restores CPU register context | Updates state enum and returns to event loop |
