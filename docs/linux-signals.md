# Linux Process Signals

POSIX signal handling: SIGTERM (15), SIGKILL (9), SIGHUP (1), SIGINT (2), and SIGSEGV (11).

## Overview
Technical cheat sheet for `linux-signals` covering production deployment, configuration, and diagnostics.

## Commands and Usage
```bash
kill -l
kill -15 <pid>
trap 'cleanup' SIGINT SIGTERM
```
