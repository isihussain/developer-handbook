# Executable Binaries: ELF Dynamic Linking with GOT & PLT

Dynamic linkers (`ld.so`) resolve external library symbol references in Position-Independent Executables (PIE) using the Procedure Linkage Table (PLT) and Global Offset Table (GOT).

```mermaid
sequenceDiagram
    participant App as Application Code
    participant PLT as PLT Entry: func@plt
    participant GOT as GOT Entry: func@got.plt
    participant Linker as Dynamic Linker: _dl_runtime_resolve
    participant Lib as Shared Library: libc.so
    App->>PLT: call func@plt
    PLT->>GOT: Jumps to address stored in GOT
    Note over GOT: First Call: Points back to PLT resolve stub!
    GOT->>Linker: Jumps to _dl_runtime_resolve(reloc_arg)
    Note over Linker: Locates func in libc.so and overwrites GOT entry
    Linker->>Lib: Executes func()
    Note over App,GOT: Subsequent Calls: PLT jumps directly to resolved func() address in GOT
```

---

## 1. Security & Relocations

* **Lazy Binding:** By default, symbol resolution is deferred until the first function call occurs.
* **Full RELRO (`-Wl,-z,relro,-z,now`):**
  - Resolves all dynamic symbols during binary load time (`DT_BIND_NOW`).
  - Marks the `.got` memory page strictly read-only (`mprotect(PROT_READ)`), preventing attackers from hijacking function pointers via GOT-overwrite exploits.
