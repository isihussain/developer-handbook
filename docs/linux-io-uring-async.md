# Linux io_uring High-Performance Asynchronous Architecture

`io_uring` provides true asynchronous system call execution via lockless ring buffers shared directly between user space and the Linux kernel via memory mapping (`mmap`). It replaces readiness notification systems (`epoll`, `kqueue`) with direct completion notification.

---

## 1. Dual Ring Buffer Architecture

`io_uring` operates two single-producer single-consumer circular ring buffers:

```
[ User Space ]                                       [ Linux Kernel ]
      |                                                     ^
      | 1. Writes SQEs to Submission Queue                  |
      v                                                     |
+-------------------------------------------------------+   |
| Submission Queue Ring (SQ)                           |   |
| - Head: updated by Kernel                             |   |
| - Tail: updated by User Space                         |   |
| Array of struct io_uring_sqe (64 bytes each)          |---+ 2. Consumes SQEs
+-------------------------------------------------------+        executes async I/O
                                                            |
+-------------------------------------------------------+   |
| Completion Queue Ring (CQ)                            |   |
| - Head: updated by User Space                         |   |
| - Tail: updated by Kernel                             |<--+ 3. Posts CQEs
| Array of struct io_uring_cqe (16 bytes each)          |
+-------------------------------------------------------+
      |
      | 4. Reads CQEs from Completion Queue
      v
[ User Space Completion Handler ]
```

- **Submission Queue Entries (`io_uring_sqe`)**: 64-byte descriptors specifying opcode (`IORING_OP_READ`, `IORING_OP_WRITEV`, `IORING_OP_SEND`, `IORING_OP_SPLICE`, etc.), file descriptor, memory buffer address, buffer length, offset, and opaque 64-bit `user_data` tag.
- **Completion Queue Entries (`io_uring_cqe`)**: 16-byte records returning matching `user_data`, signed 32-bit `res` (bytes transferred or negative errno code), and 32-bit `flags`.

---

## 2. Core System Calls

### `io_uring_setup(entries, params)`
Initializes the ring buffers with a power-of-two entry count and returns an `io_uring` file descriptor:
```c
struct io_uring_params params = {0};
int ring_fd = syscall(__NR_io_uring_setup, 1024, &params);
```

### `io_uring_enter(fd, to_submit, min_complete, flags, sig)`
Submits pending SQ entries to the kernel and optionally waits for completions:
```c
// Submit 8 SQEs, block until at least 1 CQE completes
int ret = syscall(__NR_io_uring_enter, ring_fd, 8, 1, IORING_ENTER_GETEVENTS, NULL);
```

### `io_uring_register(fd, opcode, arg, nr_args)`
Pre-registers memory buffers or file descriptors to bypass repeated per-I/O validation:
- `IORING_REGISTER_BUFFERS`: Pre-pins physical memory pages, eliminating `get_user_pages()` translation overhead during disk/network DMA.
- `IORING_REGISTER_FILES`: Pre-registers file descriptor tables, bypassing `fget()` atomic reference count increment and file table locking.

---

## 3. High-Throughput Modes

### 1. Kernel SQ Polling (`IORING_SETUP_SQPOLL`)
When instantiated with `IORING_SETUP_SQPOLL`, the kernel launches a dedicated kernel thread (`io_uring-sq`) that continuously polls the SQ ring tail:
- User space writes SQEs and increments `tail` without invoking `enter()` system calls.
- Transitions system call frequency to zero during sustained load ($100\%\text{ user-space submission}$).
- Automatically sleeps after `sq_thread_idle` milliseconds of inactivity; wakes via `IORING_ENTER_SQ_WAKEUP`.

### 2. IO Polling (`IORING_SETUP_IOPOLL`)
Bypasses hardware interrupt handlers for NVMe flash devices. The application directly polls device driver completion queues, reducing latency by avoiding hardware interrupt context switches.

### 3. Linked Submissions (`IOSQE_IO_LINK`)
Chains dependent operations sequentially:
```c
sqe[0].flags |= IOSQE_IO_LINK; // Read from socket
sqe[1].flags = 0;              // Write directly to disk
```
If `sqe[0]` fails, `sqe[1]` aborts with `-ECANCELED` without executing.

---

## 4. Architectural Comparison: io_uring vs epoll

| Metric | epoll (Reactor Model) | io_uring (Proactor Model) |
|---|---|---|
| **Programming Paradigm** | Readiness notification (Socket ready for read/write) | Completion notification (Buffer already filled/transmitted) |
| **System Call Count** | At least 2 syscalls per cycle (`epoll_wait` + `read`/`write`) | Batched 0 to 1 syscall for $N$ transfers |
| **Storage (File) I/O** | Unsupported (`epoll` always reports ready for regular files) | Fully asynchronous for all file, socket, and pipe operations |
| **Memory Transitions** | Userspace buffer copied into socket kernel buffer | Zero-copy buffer registration (`IORING_REGISTER_BUFFERS`) |
| **Thread Overhead** | Requires thread pool (`libuv`, `asio`) for blocking disk I/O | Pure kernel offloading with zero auxiliary worker threads |
| **Kernel Concurrency** | Synchronous trap on every I/O call | Asynchronous queuing with `SQPOLL` background processing |

---

## 5. Memory-Mapped Ring Access (Lockless Synchronization)

```c
// Submitting a request:
uint32_t tail = *sq_ring.tail;
uint32_t index = tail & sq_ring.ring_mask;
struct io_uring_sqe *sqe = &sqes[index];

sqe->opcode = IORING_OP_READ;
sqe->fd = file_fd;
sqe->addr = (uint64_t)buffer;
sqe->len = buffer_len;
sqe->off = offset;
sqe->user_data = request_id;

// Memory barrier ensures SQE is written before tail becomes visible to kernel
__atomic_store_n(sq_ring.tail, tail + 1, __ATOMIC_RELEASE);
```
