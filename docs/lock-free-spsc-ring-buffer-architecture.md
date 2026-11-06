# Lock-Free SPSC Ring Buffer Architecture

Single-Producer Single-Consumer (SPSC) ring buffers provide bounded, deterministic FIFO queuing between pinned threads with sub-10 nanosecond transfer overhead.

```mermaid
flowchart LR
    Producer[Producer Thread (Pinned Core 2)] -->|write data & store release head| Buffer[Circular Array Buffer]
    Consumer[Consumer Thread (Pinned Core 4)] -->|read data & store release tail| Buffer
    subgraph RingMemory[Cache-Aligned Structure]
        Head[head index: alignas 64]
        Tail[tail index: alignas 64]
    end
```

---

## 1. Cache-Line Alignment & Memory Barriers

To prevent false sharing between producer and consumer cores, the write index (`head_`) and read index (`tail_`) must reside on separate 64-byte L1 cache lines.

```cpp
#include <atomic>
#include <cstddef>
#include <new>

template <typename T, size_t Capacity>
class SpscQueue {
    static_assert((Capacity & (Capacity - 1)) == 0, "Capacity must be power of two");
public:
    SpscQueue() : head_(0), tail_(0) {}

    bool push(const T& item) {
        const size_t head = head_.load(std::memory_order_relaxed);
        const size_t tail = tail_cached_;
        
        if ((head - tail) == Capacity) {
            tail_cached_ = tail_.load(std::memory_order_acquire);
            if ((head - tail_cached_) == Capacity) {
                return false; // Queue full
            }
        }
        
        buffer_[head & (Capacity - 1)] = item;
        head_.store(head + 1, std::memory_order_release);
        return true;
    }

    bool pop(T& item) {
        const size_t tail = tail_.load(std::memory_order_relaxed);
        const size_t head = head_cached_;
        
        if (tail == head) {
            head_cached_ = head_.load(std::memory_order_acquire);
            if (tail == head_cached_) {
                return false; // Queue empty
            }
        }
        
        item = buffer_[tail & (Capacity - 1)];
        tail_.store(tail + 1, std::memory_order_release);
        return true;
    }

private:
    T buffer_[Capacity];
    
    alignas(64) std::atomic<size_t> head_;
    alignas(64) size_t tail_cached_ = 0;
    
    alignas(64) std::atomic<size_t> tail_;
    alignas(64) size_t head_cached_ = 0;
};
```
