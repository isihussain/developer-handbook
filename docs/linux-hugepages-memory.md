# Linux Transparent Hugepages (THP)

TLB miss reduction, 2MB/1GB hugepage allocation, and compaction.

## Overview
Technical cheat sheet for `linux-hugepages-memory` covering production deployment, configuration, and diagnostics.

## Commands and Usage
```bash
grep -i huge /proc/meminfo
echo always > /sys/kernel/mm/transparent_hugepage/enabled
```
