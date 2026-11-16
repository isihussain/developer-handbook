# Linux Block Layer: I/O Schedulers for NVMe vs Mechanical Disks

Legacy I/O schedulers reordered requests to minimize disk head movement. Fast NVMe SSDs perform best with multi-queue schedulers or no scheduler at all.

```mermaid
flowchart TD
    IORequest[I/O Request] --> DriveType{Storage Hardware}
    DriveType -->|High-Performance NVMe| None[none: Pure blk-mq passthrough with 0 CPU overhead]
    DriveType -->|SATA SSD with Target Latency| Kyber[kyber: Throttles queues to meet read/write latency targets]
    DriveType -->|Mechanical HDD / Desktop| BFQ[bfq: Budget Fair Queueing provides interactive desktop smoothness]
```

---

## 1. Production NVMe Best Practice
```bash
echo none | tee /sys/block/nvme*n1/queue/scheduler
```
