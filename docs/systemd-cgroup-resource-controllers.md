# systemd Resource Control Directives & Limits

systemd maps unit configuration properties directly to cgroup v2 controller interfaces.

---

## 1. Service File Directives

```ini
[Service]
# Memory limits
MemoryMin=256M
MemoryLow=512M
MemoryHigh=2G
MemoryMax=4G

# CPU limits
CPUQuota=200%
CPUSchedulingPolicy=rr

# Task & I/O limits
TasksMax=1024
IOWeight=500
```
