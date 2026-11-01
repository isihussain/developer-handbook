# SSH Certificate Authority Architecture

Short-lived signed SSH user certificates replacing authorized_keys.

## Overview
Technical cheat sheet for `ssh-certificates-ca` covering production deployment, configuration, and diagnostics.

## Commands and Usage
```bash
ssh-keygen -s ca_key -I user_alice -n alice -V +8h id_ed25519.pub
```
