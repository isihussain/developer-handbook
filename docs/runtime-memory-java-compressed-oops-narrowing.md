# JVM Internals: Compressed OOPs & 32-Bit Reference Narrowing

Java Compressed Ordinary Object Pointers (`UseCompressedOops`) compress 64-bit heap object references into 32 bits, exploiting 8-byte object alignment to address up to 32GB of heap space.

```mermaid
flowchart TD
    NarrowOOP[Compressed 32-bit Reference: r32] --> Shift[Bitwise Left Shift by 3: r32 << 3]
    BaseReg[Heap Base Register: R12] --> AddBase[Add Base Address]
    Shift --> AddBase
    AddBase --> Native64[Uncompressed 64-bit Address: R12 + (r32 << 3)]
```

---

## 1. Addressing Modes

* **Zero-Based Compressed OOPs (Heap $\le 28	ext{GB}$):** The heap starts at virtual address `0x0`; address translation uses a single shift instruction (`movq rax, [r12 + rbx*8]`) with zero base register addition.
* **Non-Zero Base (Heap $28	ext{GB} - 32	ext{GB}$):** Requires adding heap base:

$$	ext{Pointer}_{64} = 	ext{HeapBase} + (	ext{OOP}_{32} \ll 3)$$

* **32GB Ceiling:** If the heap exceeds 32GB, pointers cannot be represented in 32 bits scaled by 8, forcing JVM runtimes to fall back to uncompressed 64-bit references and increasing cache footprint by $30\% - 40\%$.
