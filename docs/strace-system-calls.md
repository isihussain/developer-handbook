# strace System Call Tracing

Intercepting and recording system calls called by a process and the signals received.

## Overview
Technical cheat sheet for `strace-system-calls` covering production deployment, configuration, and diagnostics.

## Commands and Usage
```bash
strace -f -e trace=network,file -p <pid>
```
