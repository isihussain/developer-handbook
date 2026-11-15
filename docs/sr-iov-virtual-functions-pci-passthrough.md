# SR-IOV Virtual Functions & Direct PCIe Passthrough Architecture

Single Root I/O Virtualization (SR-IOV) partitions a physical PCIe network controller (Physical Function, PF) into multiple lightweight virtual PCIe endpoints (Virtual Functions, VF) mapped directly into VM guest address spaces via VT-d IOMMU.

```mermaid
flowchart TD
    subgraph NIC[Physical PCIe Hardware NIC]
        PF[Physical Function PF0: Management & Global Config]
        VF0[Virtual Function VF0]
        VF1[Virtual Function VF1]
    end
    IOMMU[Intel VT-d / AMD-Vi IOMMU] <--> NIC
    VF0 -->|Direct DMA Passthrough: Zero Hypervisor Overhead| VM0[Guest Virtual Machine 0]
    VF1 -->|Direct DMA Passthrough: Zero Hypervisor Overhead| VM1[Guest Virtual Machine 1]
```

---

## 1. Key Performance Characteristics

* **Line-Rate Virtualization:** Reaches 100GbE line-rate without hypervisor vswitch bridge overhead.
* **IOMMU Protection:** Direct DMA isolated to guest physical memory frames (`guest-physical-address` translation).
