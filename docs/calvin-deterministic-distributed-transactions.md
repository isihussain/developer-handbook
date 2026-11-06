# Calvin Deterministic Transaction Processing Protocol

Calvin (Thomson et al.) eliminates distributed two-phase commit (2PC) stalls by executing transactions in a globally replicated deterministic sequence.

```mermaid
flowchart LR
    Tx[Transaction Requests] --> Sequencer[Global Sequencer (Paxos)]
    Sequencer --> Lock[Deterministic Lock Manager]
    Lock --> Exec[Execution Layer (No 2PC stalls)]
```
