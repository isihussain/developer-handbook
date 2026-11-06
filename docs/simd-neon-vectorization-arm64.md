# ARM64 NEON SIMD Vectorization Architecture

The ARM NEON engine executes 128-bit vector operations across 32 registers (`V0` through `V31`).

---

## 1. Lane Layouts

```c
#include <arm_neon.h>

// Vectorized addition of 4 x 32-bit floats
float32x4_t va = vld1q_f32(&a[i]);
float32x4_t vb = vld1q_f32(&b[i]);
float32x4_t vc = vaddq_f32(va, vb);
vst1q_f32(&c[i], vc);
```
