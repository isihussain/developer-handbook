# Web Performance: Largest Contentful Paint (LCP) Pipeline

LCP measures when the largest visual content element (hero image, heading text block) is rendered in the viewport.

```mermaid
flowchart TD
    HTML[Initial HTML Document Download] --> Preload["<link rel='preload' fetchpriority='high' as='image'>"]
    Preload --> HighPriorityFetch[Browser allocates immediate network bandwidth priority]
    HighPriorityFetch --> EarlyDecode[Asynchronous Image Decoding: decoding='async']
    EarlyDecode --> FastLCP[LCP Rendered in < 1.5 seconds!]
```

---

## 1. Anti-Patterns
- Lazy-loading above-the-fold hero images (`loading="lazy"`) degrades LCP by delaying discovery until layout pass.
