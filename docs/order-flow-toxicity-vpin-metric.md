# Order Flow Toxicity & VPIN Metric Computation

Volume-Synchronized Probability of Toxicity (VPIN) quantifies adverse selection risk caused by informed traders in limit order books.

```mermaid
flowchart TD
    Ticks[Continuous Trade Ticks] --> Buckets[Volume-Clock Buckets: Fixed Volume V]
    Buckets --> Classify[Tick Rule: Buy / Sell Volume Imbalance]
    Classify --> VPIN[VPIN = Sum |V_tau^B - V_tau^S| / (N * V)]
    VPIN --> Risk{VPIN > Toxicity Threshold?}
    Risk -->|Yes| Widen[Widen Spreads / Pull Maker Quotes]
    Risk -->|No| Normal[Maintain Tight Quotation Liquidity]
```

---

## 1. Mathematical Formulation

Let continuous time be discretized into volume buckets containing constant volume $V$. For bucket $\tau$, let $V_\tau^B$ be estimated buy volume and $V_\tau^S$ estimated sell volume.

$$\text{VPIN} = \frac{\sum_{\tau=1}^N |V_\tau^B - V_\tau^S|}{N \times V}$$

A spike in VPIN signals elevated order toxicity and impending inventory devaluation for passive market makers.
