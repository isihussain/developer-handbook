# Linux Core Dump Configuration & GDB

kernel.core_pattern, coredumpctl, and post-mortem analysis.

## Overview
Technical cheat sheet for `linux-core-dumps-gdb` covering production deployment, configuration, and diagnostics.

## Commands and Usage
```bash
sysctl -w kernel.core_pattern=/tmp/core.%e.%p
coredumpctl gdb
```
