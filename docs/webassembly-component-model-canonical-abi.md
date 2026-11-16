# Wasm Evolution: WebAssembly Component Model and Canonical ABI

The Wasm Component Model enables polyglot composability: combining modules written in Rust, Go, Python, and C++ into a single sandbox using WIT (Wasm Interface Type) definitions.

```mermaid
flowchart TD
    RustComponent[Rust Component] --> WIT[WIT Interface: Canonical ABI]
    GoComponent[Go Component] --> WIT
    WIT --> Lowering[Canonical Lower: Flattens complex types into i32/i64 primitives]
    Lowering --> Lifting[Canonical Lift: Reconstructs high-level types inside target module]
    Lifting --> WasmRuntime[Wasmtime Host Engine]
```

---

## 1. Memory Isolation
Components cannot access each other's linear memory directly; interaction occurs strictly across typed boundary functions governed by the Canonical ABI.
