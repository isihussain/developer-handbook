# Compiler Optimization: LLVM Tail Call Elimination & Sibling Calls

LLVM Tail Call Elimination (TCE) converts recursive tail calls into iterative loops, while Sibling Call Optimization reuses active stack frames to invoke distinct functions sharing identical return conventions.

```mermaid
flowchart TD
    subgraph TailRecursive[Tail-Recursive Function: factorial(n, acc)]
        Call[Return statement is pure tail call: factorial(n-1, n*acc)]
        Call --> TCE_Pass[LLVM TailCallElim Pass]
        TCE_Pass --> Loop["Transformed into direct jump: jmp .loop_header"]
        Loop --> NoStack["Stack Depth remains O(1) - Zero Stack Overflow!"]
    end
    subgraph SiblingCall[Sibling Call Optimization: funcA calls funcB]
        CallB[funcA returns funcB(x, y)] --> FrameCheck{Are stack arguments <= caller frame?}
        FrameCheck -->|Yes| SiblingTCE[Replaces 'call' with 'jmp funcB']
    end
```

---

## 1. Structural Requirements

1. **Clean Return:** The caller must return the callee's exact result without executing post-call operations (e.g., `return 1 + foo()` is NOT a tail call).
2. **Calling Convention Compatibility:** Caller and callee must share compatible parameter passing and stack cleanup conventions (`fastcall`, `sysv_abi`).
3. **Zero Local Address Escapes:** The caller must not pass pointers to its own stack frame variables to the callee.
