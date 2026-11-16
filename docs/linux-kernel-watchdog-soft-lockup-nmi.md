# Linux Diagnostics: Kernel Watchdog, Soft Lockups, and Hard Lockups

The Linux kernel watchdog subsystem detects deadlocks and infinite loops where kernel threads or interrupt handlers starve the CPU scheduler.

```mermaid
flowchart TD
    NMI[Hardware Non-Maskable Interrupt (NMI): Fires regardless of local_irq_disable!] --> CheckHard{Has hrtimer interrupt updated counter?}
    CheckHard -->|No for > 10s| HardLockup[HARD LOCKUP DETECTED: Kernel Panic / Core Dump]
    CheckHard -->|Yes| HRTimer[HRTimer Interrupt: Feeds watchdog thread]
    HRTimer --> SoftWatchdog[kernel watchdog/N kthread]
    SoftWatchdog --> CheckSoft{Has watchdog thread been scheduled within 20s?}
    CheckSoft -->|No| SoftLockup[SOFT LOCKUP DETECTED: BUG: soft lockup - CPU#0 stuck for 22s!]
```

---

## 1. Root Causes
- **Hard Lockup:** Core stuck inside a spinlock with interrupts disabled (`spin_lock_irqsave`).
- **Soft Lockup:** Thread running in kernel space in an infinite loop without yielding (`cond_resched()`).
