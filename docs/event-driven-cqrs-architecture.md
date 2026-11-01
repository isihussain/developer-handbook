# Command Query Responsibility Segregation (CQRS)

Separating write-side command models from read-side query projections.

## Overview
Technical cheat sheet for `event-driven-cqrs-architecture` covering production deployment, configuration, and diagnostics.

## Commands and Usage
```bash
# Command -> Event Store -> Projector -> Read DB
```
