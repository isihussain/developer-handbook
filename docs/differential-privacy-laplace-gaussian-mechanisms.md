# Data Privacy: Differential Privacy (Laplace & Gaussian Mechanisms)

Differential Privacy provides formal mathematical bounds guaranteeing that querying a dataset leaks bounded information regarding the presence or absence of any single individual record.

```mermaid
flowchart LR
    Database[Dataset D] --> Query[Analytical Query f(D)]
    Database_Neighbor[Neighboring Dataset D'] --> Query_Neighbor[Query f(D')]
    Query --> NoiseEngine[Noise Calibration Engine]
    NoiseEngine --> Output["Differentially Private Output: f(D) + Lap(Delta f / epsilon)"]
```

---

## 1. Formal Definition

A randomized algorithm $\mathcal{M}$ satisfies $(\epsilon, \delta)$-differential privacy if for all neighboring datasets $D, D'$ differing by at most one record, and all query outputs $S \subseteq 	ext{Range}(\mathcal{M})$:

$$\mathbb{P}[\mathcal{M}(D) \in S] \le e^\epsilon \cdot \mathbb{P}[\mathcal{M}(D') \in S] + \delta$$

* **Global Sensitivity ($\Delta f$):** Maximum difference $|f(D) - f(D')|_1$ across all neighboring datasets.
* **Laplace Mechanism ($\delta = 0$):** Injects noise drawn from $	ext{Lap}\left(rac{\Delta f}{\epsilon}ight)$.
* **Gaussian Mechanism ($\delta > 0$):** Injects noise drawn from $\mathcal{N}\left(0, \sigma^2ight)$ where $\sigma = rac{\Delta f \sqrt{2 \ln(1.25 / \delta)}}{\epsilon}$.
