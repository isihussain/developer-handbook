# Asynchronous I/O: POSIX AIO vs Linux io_uring

Comparison of user-space simulated asynchronous I/O versus true kernel ring buffer asynchronous system calls.

---

## 1. Architectural Comparison

| Dimension | POSIX AIO (`aio_read` / `aio_write`) | Linux `io_uring` |
|---|---|---|
| **Kernel Implementation** | User-space glibc thread pool emulation. | True kernel-space lockless ring buffers (`mmap`). |
| **Syscall Overhead** | Thread creation / context switch per request. | 0 syscalls in polling mode (`IORING_SETUP_SQPOLL`). |
| **Buffer Management** | User buffer copy or pipe handoff. | Fixed registered buffers (`IORING_REGISTER_BUFFERS`). |
| **Opcode Support** | Disk file read/write only. | Full syscall suite: sockets, disk, accept, connect, splice. |
