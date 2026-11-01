# Valgrind Memcheck & Memory Profiling

Detecting buffer overflows, use-after-free, uninitialized memory reads, and memory leaks.

## Overview
Technical cheat sheet for `valgrind-memcheck` covering production deployment, configuration, and diagnostics.

## Commands and Usage
```bash
valgrind --leak-check=full --track-origins=yes ./binary
```
