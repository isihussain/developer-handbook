# ELF Dynamic Linking: PLT and GOT

Procedure Linkage Table and Global Offset Table lazy symbol binding.

## Overview
Technical cheat sheet for `dynamic-linker-elf-plt-got` covering production deployment, configuration, and diagnostics.

## Commands and Usage
```bash
readelf -r binary
objdump -d -j .plt binary
```
