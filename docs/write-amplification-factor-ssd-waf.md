# Storage Hardware: Write Amplification Factor (WAF) and SSD Endurance

SSDs cannot overwrite flash memory in-place; they write in pages (4KB-16KB) and erase in blocks (2MB-8MB), causing internal garbage collection overhead.

```mermaid
flowchart TD
    AppWrite[Application writes 4KB record] --> SSDController[SSD Flash Translation Layer FTL]
    SSDController --> BlockErase[Must read, erase, and rewrite full 4MB Block!]
    BlockErase --> FlashWear["Actual Bytes Written to NAND Flash = 40KB (WAF = 10x!)"]
```

---

## 1. WAF Formula
$$\text{WAF} = \frac{\text{Bytes Written to NAND Flash}}{\text{Bytes Written by Host Application}}$$
Sequential writes (LSM WAL) achieve WAF $\approx 1.0$, while random in-place updates can spike WAF above $15.0$, rapidly burning out SSD drive write endurance.
