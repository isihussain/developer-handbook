# Linux Security Hardening (CIS Benchmark)

Disabling unused filesystems, sysctl network hardening, and auditd rules.

## Overview
Technical cheat sheet for `linux-hardening-benchmarks` covering production deployment, configuration, and diagnostics.

## Commands and Usage
```bash
sysctl -w net.ipv4.conf.all.rp_filter=1
auditctl -l
```
