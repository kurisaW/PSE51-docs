# sched_get_priority_max, sched_get_priority_min

## SYNOPSIS

```c
#include <sched.h>

int sched_get_priority_max(int policy);
int sched_get_priority_min(int policy);
```

## DESCRIPTION

The `sched_get_priority_max()` and `sched_get_priority_min()` functions shall return the appropriate maximum or minimum, respectively, for the scheduling policy specified by `policy`.

The value of `policy` shall be one of the scheduling policy values defined in `<sched.h>`.

## RETURN VALUE

If successful, the `sched_get_priority_max()` and `sched_get_priority_min()` functions shall return the appropriate maximum or minimum values, respectively. If unsuccessful, they shall return a value of -1 and set `errno` to indicate the error.

## ERRORS

The `sched_get_priority_max()` and `sched_get_priority_min()` functions shall fail if:

- **EINVAL**
  - The value of the `policy` parameter does not represent a defined scheduling policy.

---

*The following sections are informative.*

## EXAMPLES

None.

## APPLICATION USAGE

None.

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- [`sched_getparam()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sched_getparam.html)
- [`sched_setparam()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sched_setparam.html)
- [`sched_getscheduler()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sched_getscheduler.html)
- [`sched_rr_get_interval()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sched_rr_get_interval.html)
- [`sched_setscheduler()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sched_setscheduler.html)

XBD [`<sched.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/sched.h.html)

## CHANGE HISTORY

### First released in Issue 5
Included for alignment with the POSIX Realtime Extension.

### Issue 6
These functions are marked as part of the Process Scheduling option.

The `[ENOSYS]` error condition has been removed as stubs need not be provided if an implementation does not support the Process Scheduling option.

The `[ESRCH]` error condition has been removed since these functions do not take a `pid` argument.

IEEE Std 1003.1-2001/Cor 1-2002, item XSH/TC1/D6/52 is applied, changing the PS margin code in the SYNOPSIS to PS|TPS.

---
