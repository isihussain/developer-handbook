# Peer-to-Peer Economics: BitTorrent Tit-For-Tat Choking Algorithm

BitTorrent incentivizes peer bandwidth sharing through a game-theoretic Tit-For-Tat strategy: unchoking the 4 fastest uploading peers while periodically optimistic unchoking.

```mermaid
flowchart TD
    PeerList[Connected Peers] --> Rank[Rank Peers by Upload Bandwidth to Us]
    Rank --> UnchokeTop4[Unchoke Top 4 Fastest Uploaders: Reward good behavior!]
    Rank --> ChokeOthers[Choke remaining peers: Stop sending data]
    subgraph OptimisticUnchoke
        Timer[Every 30 Seconds] --> PickRandom[Optimistically Unchoke 1 Random Peer!]
        PickRandom --> Discover[Discovers new high-speed peers & allows newcomers to bootstrap!]
    end
```

---

## 1. Free-Rider Prevention
Peers that throttle their upload speed are choked by the swarm, naturally receiving lower download speeds.
