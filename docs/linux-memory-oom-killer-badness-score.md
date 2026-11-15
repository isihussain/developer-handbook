# Linux Kernel: Out-Of-Memory (OOM) Killer & Badness Score

When physical memory and swap are exhausted, the Linux kernel Out-Of-Memory (OOM) Killer evaluates all running processes, calculating a normalized `badness` score to terminate the highest-consuming rogue process.

```mermaid
flowchart TD
    OOM[Kernel alloc_pages() fails: Out of Memory] --> Scan[Scans all task_struct processes]
    Scan --> Badness[Calculate oom_badness score: 0 to 1000]
    Badness --> Formula["Points = RSS + Swap Pages + Page Table Pages"]
    Formula --> Adj[Adjust by /proc/PID/oom_score_adj]
    Adj --> Victim[Select Task with Highest Score: Issues SIGKILL]
```

---

## 1. Score Calculation Formula

$$	ext{badness} = rac{	ext{Resident Set Size (RSS)} + 	ext{Swap Pages} + 	ext{PTE Pages}}{	ext{Total Physical RAM}} 	imes 1000$$

* **`oom_score_adj` Tuning:** Values range from `-1000` (completely immune; never killed by OOM) to `+1000` (always preferred victim).
* **Root Exemption:** Historically, root-owned processes received modest score deductions; modern kernels rely strictly on exact memory consumption metrics and container boundaries.
