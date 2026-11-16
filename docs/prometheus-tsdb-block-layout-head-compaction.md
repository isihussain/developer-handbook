# Observability Storage: Prometheus TSDB Block Layout

Prometheus stores time-series samples in two-hour immutable blocks containing data chunks, metadata, and an inverted index.

```mermaid
flowchart TD
    Scrape[Scrape Targets: Samples arriving every 15s] --> Head[Head Block: In-Memory Ring Buffer + WAL]
    Head -->|Every 2 Hours| ImmutableBlock[Immutable Block on Disk: 2h Chunk]
    ImmutableBlock --> Chunks[chunks/ directory: Compressed Gorilla/XOR Samples]
    ImmutableBlock --> Index[index file: Inverted Label Index + Posting Lists]
    ImmutableBlock --> Compactor[Compactor Daemon: Merges 2h blocks into 8h / 24h blocks]
```

---

## 1. Gorilla Floating-Point Compression
Prometheus utilizes Facebook's Gorilla compression algorithm:
- Timestamps compressed using delta-of-deltas (~1.37 bytes per timestamp).
- Values compressed using XOR of floating-point representations.
