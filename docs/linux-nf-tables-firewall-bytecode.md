# Linux nftables Architecture & Bytecode Evaluation

`nftables` replaces iptables with an in-kernel register-based virtual machine evaluating packet payloads.

```mermaid
flowchart LR
    Packet[Packet Header] --> Reg[Registers 0..15]
    Reg --> VM[nftables Bytecode VM]
    VM --> Verdict{Rule Verdict}
    Verdict -->|accept| Accept[Pass to Stack]
    Verdict -->|drop| Drop[Discard Packet]
```

---

## 1. Architecture

- Unified tool and kernel module for IPv4, IPv6, ARP, and bridging.
- Atomic rule set updates without table flushing.
- Native set and dictionary indexing ($O(1)$ lookups for IP lists).
