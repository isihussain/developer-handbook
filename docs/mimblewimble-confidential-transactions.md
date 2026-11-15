# Mimblewimble: Cut-Through Pruning & Confidential Transactions

Mimblewimble eliminates Bitcoin-style scripts and addresses, representing all transactions as Pedersen commitments and verifying blockchain history purely through transaction kernel excesses.

```mermaid
flowchart LR
    subgraph Block Transactions
        Tx1[Tx 1: Input A -> Output B]
        Tx2[Tx 2: Input B -> Output C]
    end
    Block[Cut-Through Block Pruning] --> Pruned[Pruned Block: Input A -> Output C]
    Pruned -.->|Output B completely deleted from blockchain history| Storage[Minimal Disk Footprint]
```

---

## 1. Kernel Excess Invariant

A transaction kernel contains the excess public key $E = \sum C_{	ext{out}} - \sum C_{	ext{in}}$ and a signature proving knowledge of the private blinding factor sum, proving the transaction creates no new coins from thin air.
