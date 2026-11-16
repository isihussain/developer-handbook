# Cloud Infrastructure: AWS Firecracker MicroVM Architecture

Firecracker powers AWS Lambda and Fargate, combining the security and isolation of hardware virtualization (KVM) with the fast launch times of containers (<5ms).

```mermaid
flowchart TD
    Host[Linux Bare-Metal Host] --> KVM[Linux Kernel KVM Subsystem]
    KVM --> Jailer[Firecracker Jailer: cgroups, chroot, seccomp, drop privileges]
    Jailer --> VMM[Firecracker Minimal VMM in Rust]
    VMM --> Devices[Minimal Devices: virtio-net, virtio-block, virtio-vsock, serial console]
    VMM --> GuestKernel[Custom Minimal Guest Linux Kernel (<5MB memory footprint)]
```

---

## 1. Stripped Hardware Emulation
Firecracker deliberately eliminates legacy QEMU PCI buses, IDE controllers, and ACPI tables, reducing attack surfaces and enabling thousands of microVMs per host.
