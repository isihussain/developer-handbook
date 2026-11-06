# Linux TCP CUBIC Congestion Control Architecture

CUBIC is the default congestion control algorithm in Linux, replacing linear AIMD growth with a cubic polynomial function.

---

## 1. Window Growth Function

$$W_{	ext{cubic}}(t) = C (t - K)^3 + W_{	ext{max}}$$
- $W_{	ext{max}}$: Congestion window size prior to previous packet loss.
- $K = \sqrt[3]{rac{W_{	ext{max}} eta}{C}}$: Time required to restore window back to $W_{	ext{max}}$.
- $C$: Scaling constant ($0.4$).
- Eliminates RTT unfairness on heterogeneous network paths.
