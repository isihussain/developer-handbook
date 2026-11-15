# Linux I/O: io_uring Registered Fixed Buffers & File Descriptors

The `io_uring` subsystem eliminates per-I/O virtual memory mapping and atomic file reference operations through pre-registered buffers (`IORING_REGISTER_BUFFERS`) and files (`IORING_REGISTER_FILES`).

```mermaid
flowchart TD
    App[Application Process] -->|io_uring_register: IORING_REGISTER_BUFFERS| Kernel[Linux Kernel io_uring]
    Kernel -->|Pins physical pages in RAM: get_user_pages| PinnedMem[Pre-Pinned Physical Pages]
    App -->|io_uring_register: IORING_REGISTER_FILES| FileTable[Registered Direct File Table]
    Note over App,FileTable: Subsequent SQEs use IORING_OP_READ_FIXED & IOSQE_FIXED_FILE<br/>Bypasses fget()/fput() atomics and page table translation walks
```

---

## 1. Architectural Mitigations

* **Registered Buffers (`IORING_REGISTER_BUFFERS`):** Physical memory pages are pinned once during setup. The kernel skips `get_user_pages()` calls during submission loops.
* **Registered Files (`IORING_REGISTER_FILES`):** Avoids acquiring atomic locks on `files_struct` and invoking `fget()` / `fput()` per system call, which creates contention under heavy multi-threading.
* **Direct Descriptors:** In modern kernels, files can be opened directly into the fixed table (`IORING_OP_OPENAT_DIRECT`), bypassing the process file descriptor table completely.
