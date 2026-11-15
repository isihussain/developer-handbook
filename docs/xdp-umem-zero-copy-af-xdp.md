# AF_XDP (XSK) Zero-Copy UMEM Frame Architecture

AF_XDP bypasses the Linux network stack by mapping registered physical RAM buffers (UMEM) directly into user space, allowing direct memory exchange between hardware DMA rings and software queues.

```mermaid
flowchart TD
    subgraph UMEM Virtual Memory
        F0[Frame 0]
        F1[Frame 1]
        F2[Frame 2]
    end
    subgraph XSK Rings
        FillRing[Fill Ring: Userspace gives empty frames to NIC]
        RxRing[Rx Ring: NIC returns filled packets to Userspace]
        TxRing[Tx Ring: Userspace gives outgoing packets]
        CompRing[Completion Ring: NIC confirms transmission]
    end
    NIC[Hardware NIC DMA Engine] <--> FillRing
    NIC <--> RxRing
    NIC <--> TxRing
    NIC <--> CompRing
```

---

## 1. Zero-Copy Setup Flags

* `XDP_ZERO_COPY`: Enforces true zero-copy hardware DMA direct to UMEM.
* `XDP_USE_NEED_WAKEUP`: Avoids redundant `poll()` / `sendto()` syscalls when driver queues are active.
