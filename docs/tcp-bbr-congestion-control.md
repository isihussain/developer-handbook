# BBR Congestion Control (Bottleneck Bandwidth and RTT)

Replacing loss-based congestion control (CUBIC) with model-based pacing.

## Overview
Technical cheat sheet for `tcp-bbr-congestion-control` covering production deployment, configuration, and diagnostics.

## Commands and Usage
```bash
sysctl -w net.ipv4.tcp_congestion_control=bbr
```
