# Differential Flame Graphs Profiling

Differential flame graphs visualize performance regressions and improvements across two system profiles.

```
[ Visual Legend ]
- Blue Bars: Performance improvement (reduced execution sample count).
- Red Bars:  Performance regression (increased execution sample count).
- Width:     Proportional to CPU time consumed on the stack.
```

---

## 1. Generation Pipeline

```bash
# Fold stack traces
perf script | stackcollapse-perf.pl > baseline.folded
perf script | stackcollapse-perf.pl > after.folded

# Generate differential flame graph
difffolded.pl baseline.folded after.folded | flamegraph.pl > diff.svg
```
