# Go Internals: The G-M-P Work-Stealing Scheduler

The Go runtime multiplexes $G$ (goroutines) onto $M$ (OS kernel threads) through $P$ (logical processors representing hardware CPU cores).

```mermaid
flowchart TD
    P1[Logical Processor P1] --> LRQ1[Local Run Queue: Up to 256 Goroutines]
    P2[Logical Processor P2: Empty Queue!] --> Steal[Work-Stealing: Steals half of P1 LRQ!]
    Steal --> LRQ1
    Syscall[Goroutine executes blocking Syscall] --> Detach[M detaches from P; new M created to service P]
```

---

## 1. Preemption Mechanics
- Pre-Go 1.14: Cooperative preemption on function call prologue checks (`morestack`).
- Modern Go: Non-cooperative asynchronous preemption via OS POSIX signals (`SIGURG`).
