# Ultra-Low Latency: RDMA InfiniBand Atomic Fetch-and-Add & Compare-and-Swap

Remote Direct Memory Access (RDMA) over InfiniBand and RoCEv2 supports hardware-enforced remote atomic operations, allowing nodes to modify 64-bit remote host memory without interrupting remote CPUs.

```mermaid
sequenceDiagram
    participant LocalHost as Local Host CPU
    participant LocalHCA as Local RDMA HCA (NIC)
    participant RemoteHCA as Remote RDMA HCA (NIC)
    participant RemoteRAM as Remote Host Physical RAM
    LocalHost->>LocalHCA: Post Send WR: IBV_WR_ATOMIC_FETCH_AND_ADD
    LocalHCA->>RemoteHCA: PCIe RDMA Request Packet (Target Virtual Address + Add Value)
    Note over RemoteHCA: Hardware Memory Controller executes atomic fetch-and-add on bus
    RemoteHCA->>RemoteRAM: Atomic Read-Modify-Write in 64-bit word
    RemoteHCA->>LocalHCA: RDMA Atomic Acknowledge (Contains original remote value)
    LocalHCA->>LocalHost: Post Completion Queue Entry (CQE)
```

---

## 1. Remote Atomic Invariants

* **Supported Operations:**
  - `IBV_WR_ATOMIC_FETCH_AND_ADD`: Atomically adds a 64-bit value to target memory location and returns previous value.
  - `IBV_WR_ATOMIC_CMP_AND_SWP`: Atomically compares memory against `compare_add` and swaps with `swap` if equal.
* **Alignment Constraint:** Remote target addresses must be strictly 8-byte aligned (64-bit boundaries).
* **Zero Remote CPU Intervention:** Remote CPU cores continue processing application workloads while the HCA memory controller executes the transaction.
