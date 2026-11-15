# NIC Receive Side Scaling (RSS) & Flow Director Steering

Hardware RSS hashes incoming 4-tuple IP packet headers across RX queues, while Intel Flow Director (FDir) applies deterministic hardware flow rules to pin specific connections to dedicated CPU cores.

```mermaid
flowchart TD
    Packet[Incoming Packet: SrcIP, DstIP, SrcPort, DstPort] --> Parse[NIC Hardware Parser]
    Parse --> RuleCheck{Flow Director Rule Matches?}
    RuleCheck -->|Match: Deterministic Rule| TargetQueue[Direct to Specific RX Queue & CPU Core]
    RuleCheck -->|No Match: Toeplitz Hash| Hash[Compute 32-bit Toeplitz Hash Key]
    Hash --> Indirection[Indirection Table: modulo Queue Count]
    Indirection --> RSSQueue[Distributed RX Queue Pool]
```

---

## 1. Toeplitz Hash Invariant

The Toeplitz hash function ensures symmetric hashing: forward and reverse directions of a TCP flow hash to identical queue indices, keeping connection states localized to a single CPU L1 cache.
