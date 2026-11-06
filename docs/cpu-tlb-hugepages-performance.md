# CPU TLB Performance: 2MB vs 1GB HugePages

Standard 4KB virtual memory pages strain CPU Translation Lookaside Buffers (TLB) on large-memory database instances.

---

## 1. TLB Coverage

- 512 L1 TLB entries covering 4KB pages = **2 MB coverage**.
- 512 L1 TLB entries covering 2MB hugepages = **1 GB coverage**.
- Hugepages eliminate multi-level page table walks, reducing memory latency by 10% to 25%.
