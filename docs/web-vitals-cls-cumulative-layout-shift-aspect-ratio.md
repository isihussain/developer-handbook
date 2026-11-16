# Web Performance: Cumulative Layout Shift (CLS) Prevention

CLS measures unexpected layout movements during page lifecycle, calculated as $\text{Impact Fraction} \times \text{Distance Fraction}$.

```mermaid
flowchart LR
    ImageWithoutDim[Image tag without width/height attributes] --> DynamicLoad[Image loads late: pushes content down!]
    DynamicLoad --> HighCLS[CLS Spikes above 0.25: Poor Experience!]
    subgraph Solution
        ReservedSpace["CSS: aspect-ratio: 16 / 9; width: 100%;"] --> ZeroShift[Browser reserves space before image loads: CLS = 0.00!]
    end
```

---

## 1. Target
Google Core Web Vitals benchmark requires $CLS \le 0.1$ at the 75th percentile of page visits.
