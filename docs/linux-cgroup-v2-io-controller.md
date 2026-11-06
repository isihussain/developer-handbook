# Linux cgroup v2 I/O Controller & Throttling

Limits storage bandwidth and assigns fair queuing weights across container workloads under `/sys/fs/cgroup`.

---

## 1. Configuration Directives

```bash
# Set proportional I/O weight (1..10000, default 100)
echo "default 300" > /sys/fs/cgroup/production.slice/io.weight

# Set hard read/write limits on block device (8:0 = /dev/sda)
# Max 50 MB/s read, 25 MB/s write, 2000 IOPS
echo "8:0 rbps=52428800 wbps=26214400 riops=2000 wiops=1000" > /sys/fs/cgroup/sandbox.slice/io.max
```
