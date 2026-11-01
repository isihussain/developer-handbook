# PromQL Metrics & Alerting Engine

Counters, Gauges, Histograms, rate() vs irate(), and quantile estimation.

## Overview
Technical cheat sheet for `prometheus-promql-metrics` covering production deployment, configuration, and diagnostics.

## Commands and Usage
```bash
rate(http_requests_total[5m])
histogram_quantile(0.99, sum(rate(http_request_duration_seconds_bucket[5m])) by (le))
```
