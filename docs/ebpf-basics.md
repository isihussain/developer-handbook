# Extended Berkeley Packet Filter (eBPF)

Kernel-level observability, network filtering, and security tracing without modifying kernel source.

## Overview
Technical cheat sheet for `ebpf-basics` covering production deployment, configuration, and diagnostics.

## Commands and Usage
```bash
bpftool prog list
bpftrace -e 'tracepoint:syscalls:sys_enter_openat { printf("%s\n", str(args->filename)); }'
```
