# Ticket Spinlock FIFO Fairness Mechanics

Ticket spinlocks eliminate CPU core starvation in SMP systems by providing first-in, first-out (FIFO) acquisition order through atomic fetch-and-add ticket and serving counters.

```mermaid
flowchart LR
    subgraph Lock State
        Next[next_ticket: 4]
        Now[now_serving: 2]
    end
    Core1[Core 1: Holds Ticket 2] -->|Matching now_serving| Critical[Execute Critical Section]
    Core2[Core 2: Holds Ticket 3] -.->|Spins waiting for now_serving == 3| Lock State
    Core3[Core 3: Takes Ticket 4 via FAA] -.->|Spins waiting for now_serving == 4| Lock State
    Critical --> Release[Unlock: Increment now_serving to 3]
```

---

## 1. Data Structure Layout

```c
typedef struct {
    union {
        unsigned int slock;
        struct __raw_tickets {
            unsigned short owner; // now_serving
            unsigned short next;  // next_ticket
        } tickets;
    };
} arch_spinlock_t;
```
