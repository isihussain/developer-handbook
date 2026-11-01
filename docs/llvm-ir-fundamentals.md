# LLVM Intermediate Representation (IR)

Basic blocks, phi nodes, type system, memory operations (alloca, load, store), and opt passes.

## Overview
Technical cheat sheet for `llvm-ir-fundamentals` covering production deployment, configuration, and diagnostics.

## Commands and Usage
```bash
clang -S -emit-llvm source.c -o source.ll
opt -O3 -S source.ll -o optimized.ll
```
