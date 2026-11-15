# Cryptography: SPDZ Multi-Party Computation & Beaver Triples

The SPDZ protocol enables $N$ mutually distrusting parties to jointly compute arbitrary arithmetic circuits over a finite field with active security against up to $N - 1$ malicious corruptions using precomputed Beaver multiplication triples.

```mermaid
flowchart LR
    subgraph OfflinePhase[Offline Preprocessing: Zero-Knowledge / OT]
        Triples["Generate Beaver Triples: [a], [b], [c] where c = a * b"]
        MACs["Generate Authenticated MAC Shares: [m] = alpha * m"]
    end
    subgraph OnlinePhase[Online Phase: Ultra-Fast Linear Secret Sharing]
        InputShares["Parties hold linear shares [x] and [y]"]
        OpenDiff["Reveal epsilon = x - a, delta = y - b"]
        ComputeMult["Compute [z] = [c] + epsilon*[b] + delta*[a] + epsilon*delta"]
    end
    OfflinePhase --> OnlinePhase
```

---

## 1. Beaver Multiplication Steps

To multiply two secret-shared values $[x]$ and $[y]$ using a preprocessed triple $([a], [b], [c])$:
1. Each party locally computes $[\epsilon] = [x] - [a]$ and $[\delta] = [y] - [b]$.
2. All parties broadcast and reconstruct $\epsilon$ and $\delta$ in the clear.
3. Parties compute share of product $[z] = [x \cdot y]$ linearly without further network rounds:

$$[z] = [c] + \epsilon [b] + \delta [a] + \epsilon \delta$$
