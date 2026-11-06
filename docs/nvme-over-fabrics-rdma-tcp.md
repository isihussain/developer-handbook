# NVMe-over-Fabrics (NVMe-oF): RDMA vs TCP Transport

Extends the NVMe storage protocol across network fabrics, bypassing SCSI translation layers.

---

## 1. Transport Comparison

- **NVMe/RDMA (RoCEv2 / InfiniBand)**: Zero-copy kernel bypass directly from host memory into remote NVMe controller queues. Sub-10 microsecond latency.
- **NVMe/TCP**: Operates over standard datacenter Ethernet switches without requiring lossless Converged Ethernet (PFC/ECN).
