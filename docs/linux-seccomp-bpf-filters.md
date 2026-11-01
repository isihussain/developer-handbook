# Linux Secure Computing Mode (seccomp)

Restricting process system call attack surface via BPF bytecode.

## Overview
Technical cheat sheet for `linux-seccomp-bpf-filters` covering production deployment, configuration, and diagnostics.

## Commands and Usage
```bash
cat /proc/sys/kernel/seccomp/actions_avail
```
