# Web Performance: Core Web Vitals INP Optimization Architecture

Interaction to Next Paint (INP) measures the longest latency between user interaction (click, keypress, tap) and the next frame update.

```mermaid
flowchart LR
    UserClick[User Clicks Button] --> InputDelay[1. Input Delay: Waiting for main thread to idle]
    InputDelay --> ProcessingTime[2. Processing Time: JavaScript Event Handlers Execute]
    ProcessingTime --> PresentationDelay[3. Presentation Delay: Recalculate Style, Layout & Paint]
    PresentationDelay --> NextFrame[Next Frame Renders on Screen: Total INP Duration]
```

---

## 1. Thresholds
- **Good:** $\le 200\text{ms}$
- **Needs Improvement:** $200\text{ms} - 500\text{ms}$
- **Poor:** $> 500\text{ms}$
