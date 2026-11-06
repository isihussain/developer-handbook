# Linux Landlock LSM Sandboxing (Kernel 5.13+)

Landlock allows unprivileged applications to enforce mandatory access controls (filesystem and network) on themselves.

---

## 1. Landlock Ruleset Workflow

```c
#include <linux/landlock.h>
#include <sys/prctl.h>

// 1. Create Landlock ruleset
int ruleset_fd = landlock_create_ruleset(&attr, sizeof(attr), 0);

// 2. Add path benefit rules
landlock_add_rule(ruleset_fd, LANDLOCK_RULE_TYPE_PATH_BENEATH, &path_beneath, 0);

// 3. Disable future privilege escalation
prctl(PR_SET_NO_NEW_PRIVS, 1, 0, 0, 0);

// 4. Enforce sandboxing on self and all child threads
landlock_restrict_self(ruleset_fd, 0);
```
