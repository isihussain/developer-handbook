# Apache Arrow: IPC Streaming & Zero-Copy Flatbuffers

Apache Arrow defines a standardized language-independent in-memory columnar format, enabling shared-memory data transfer between processes without serialization overhead.

```mermaid
flowchart TD
    ProcessA[Process A: Python / Pandas] -->|Share POSIX Shared Memory / shm_open| ShmPool[Shared Memory Buffer: Arrow RecordBatch]
    ShmPool --> ProcessB[Process B: Rust / DataFusion]
    Note over ProcessA,ProcessB: Zero Serialization / Zero Deserialization Overhead
```

---

## 1. Memory Layout Specification

* **Contiguous Buffers:** Each array contains a validity bitmap buffer, an offset buffer (for variable-width items like strings), and a contiguous data buffer.
* **Flatbuffers Metadata:** Schema definitions, dictionary mappings, and batch dimensions are serialized using Google Flatbuffers at the head of the IPC stream.
* **Direct Pointer Access:** Consuming engines read primitive columns directly by mapping raw memory addresses to CPU vector registers.
