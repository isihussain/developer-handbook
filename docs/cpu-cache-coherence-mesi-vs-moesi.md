# Computer Architecture: MESI and MOESI Cache Coherence Protocols

Multi-core processors maintain consistent memory views across private L1/L2 caches using bus snooping coherence protocols.

```mermaid
stateDiagram-v2
    [*] --> Modified: Write to Exclusive/Shared Cacheline
    Modified --> Owner: MOESI: Read Request from other core without writing to DRAM
    Modified --> Shared: Bus Read detected
    Shared --> Invalid: Invalidation Broadcast from another writer
    Exclusive --> Modified: Silent Write (0 bus traffic)
    Invalid --> Shared: Fetch from RAM or Owner Core
```

---

## 1. MOESI Owner State
The **Owner (O)** state in AMD and ARM processors allows a core to share dirty modified data directly with other cores without writing it back to slow DDR memory.
