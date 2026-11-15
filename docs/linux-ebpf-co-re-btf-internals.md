# eBPF: Compile Once - Run Everywhere (CO-RE) & BTF Relocations

eBPF CO-RE eliminates runtime Clang compiler dependencies on production servers by embedding BPF Type Format (BTF) debug data and resolving kernel field offset relocations at load time.

```mermaid
flowchart LR
    DevMachine[Developer Machine: Clang -target bpf -g] --> BPF_ELF[Compiled eBPF Object File with BTF Relocations]
    BPF_ELF --> LibbpfLoader[Production Node: libbpf Loader]
    KernelBTF[/sys/kernel/btf/vmlinux] --> LibbpfLoader
    LibbpfLoader -->|Rewrites bytecode struct offsets to match target kernel| KernelVerifier[Linux Kernel Verifier]
```

---

## 1. Relocation Mechanics

* **Clang Builtins:** `__builtin_preserve_access_index()` marks struct field accesses, generating BTF relocation records in the `.rel.BTF` ELF section.
* **Loader Adjustment:** When loading bytecode, `libbpf` matches source BTF against target `/sys/kernel/btf/vmlinux`, adjusting instruction immediate offsets dynamically.
