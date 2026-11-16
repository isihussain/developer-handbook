# Linux Memory: Out-Of-Memory (OOM) Killer Scoring Algorithm

When physical RAM and swap are completely exhausted, the kernel's `oom_badness()` function computes a score from 0 to 1000 to determine which task to terminate.

```mermaid
flowchart TD
    MemoryExhausted[Physical RAM & Swap = 0] --> OOM_Scan[Scan Process Table]
    OOM_Scan --> Calc["Base Points = (RSS + Swap + PageTablePages) / TotalRAM * 1000"]
    Calc --> Adjust["Adjust with /proc/[pid]/oom_score_adj (-1000 to +1000)"]
    Adjust --> HighestScore[Select Task with Highest Final Score]
    HighestScore --> SIGKILL[Send SIGKILL (kill -9) to Target Process]
```

---

## 1. Immunity Flag
Writing `-1000` to `/proc/[pid]/oom_score_adj` completely exempts critical daemons (e.g. sshd, kubelet) from OOM termination.
