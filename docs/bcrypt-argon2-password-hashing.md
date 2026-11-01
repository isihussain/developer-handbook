# Argon2id & Bcrypt Password Hashing

Memory-hard functions resistant to GPU and ASIC brute-force attacks.

## Overview
Technical cheat sheet for `bcrypt-argon2-password-hashing` covering production deployment, configuration, and diagnostics.

## Commands and Usage
```bash
python3 -c 'import argon2; print(argon2.PasswordHasher().hash("pass"))'
```
