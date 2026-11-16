# Linux Filesystem: flock() vs fcntl() Byte-Range Record Locking

Linux provides advisory file locking through `flock()` (whole file) and `fcntl()` (byte ranges), each with distinct inheritance and closure rules.

```mermaid
flowchart TD
    LockType -->|flock(fd, LOCK_EX)| WholeFile[Associated with open file table description: Survives dup(), inherited across fork()]
    LockType -->|fcntl(fd, F_SETLK)| RangeLock[Byte-Range Lock: Associated with (PID, Inode)]
    RangeLock --> Gotcha["CAVEAT: Closing ANY file descriptor to the file releases ALL fcntl locks held by the process!"]
```

---

## 1. OFD Locks (Open File Description)
Linux 3.15 introduced `F_OFD_SETLK`, combining range locking with `flock()` ownership semantics, safe for multi-threaded applications.
