# Confidential Computing: Path ORAM (Oblivious RAM)

Oblivious RAM (ORAM) eliminates access pattern leakage on untrusted storage providers by randomizing memory locations and re-encrypting blocks on every read and write operation.

```mermaid
flowchart TD
    subgraph PositionMap[Client Secure Cache]
        PosMap[Position Map: Block ID -> Leaf ID]
        Stash[Local Stash Memory]
    end
    subgraph ServerTree[Untrusted Server: Binary Tree of Buckets]
        Root[Bucket 0: Root] --> B1[Bucket 1]
        Root --> B2[Bucket 2]
        B1 --> L1[Leaf 1]
        B1 --> L2[Leaf 2]
        B2 --> L3[Leaf 3]
        B2 --> L4[Leaf 4]
    end
    PosMap -->|Read Path from Root to Leaf| Root
```

---

## 1. Path ORAM Operation Cycle

1. **Lookup:** Client consults local Position Map to determine leaf node $L$ assigned to target block $a$.
2. **Read Path:** Client reads and decrypts all buckets along the path from Root to Leaf $L$ into its local Stash.
3. **Remap:** Client assigns block $a$ a uniformly random new leaf $L' \leftarrow 	ext{Random}()$ in the Position Map.
4. **Write Path:** Client writes blocks from Stash back to the path toward $L$ as far down the tree as possible, padding remaining bucket slots with encrypted dummy blocks.
