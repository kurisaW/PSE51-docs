# sched_rr_get_interval — get execution time limits (REALTIME)

## SYNOPSIS

```c
#include <sched.h>

int sched_rr_get_interval(pid_t pid, struct timespec *interval);
```

## DESCRIPTION

The `sched_rr_get_interval()` function shall update the `timespec` structure referenced by the `interval` argument to contain the current execution time limit (that is, time quantum) for the process specified by `pid`. If `pid` is zero, the current execution time limit for the calling process shall be returned.

## RETURN VALUE

If successful, the `sched_rr_get_interval()` function shall return zero. Otherwise, it shall return a value of -1 and set `errno` to indicate the error.

## ERRORS

The `sched_rr_get_interval()` function shall fail if:

- **[ESRCH]** - No process can be found corresponding to that specified by `pid`.

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
- [`sched_get_priority_max()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sched_get_priority_max.html)
- [`sched_getscheduler()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sched_getscheduler.html)
- [`sched_setparam()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sched_setparam.html)
- [`sched_setscheduler()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sched_setscheduler.html)

XBD [`<sched.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/sched.h.html)

## CHANGE HISTORY

### First released in Issue 5. Included for alignment with the POSIX Realtime Extension.

### Issue 6

The `sched_rr_get_interval()` function is marked as part of the Process Scheduling option.

The `[ENOSYS]` error condition has been removed as stubs need not be provided if an implementation does not support the Process Scheduling option.

IEEE Std 1003.1-2001/Cor 1-2002, XSH/TC1/D6/53 is applied, changing the PS margin code in the SYNOPSIS to PS|TPS.

---
