# pthread_getcpuclockid

The Open Group Base Specifications Issue 8
IEEE Std 1003.1-2024
Copyright © 2001-2024 The IEEE and The Open Group

---

## NAME

pthread_getcpuclockid — access a thread CPU-time clock (ADVANCED REALTIME THREADS)

## SYNOPSIS

```c
#include <pthread.h>
#include <time.h>

int pthread_getcpuclockid(pthread_t thread_id, clockid_t *clock_id);
```

## DESCRIPTION

The `pthread_getcpuclockid()` function shall return in `clock_id` the clock ID of the CPU-time clock of the thread specified by `thread_id`, if the thread specified by `thread_id` exists.

## RETURN VALUE

Upon successful completion, `pthread_getcpuclockid()` shall return zero; otherwise, an error number shall be returned to indicate the error.

## ERRORS

No errors are defined.

---

*The following sections are informative.*

## EXAMPLES

None.

## APPLICATION USAGE

The `pthread_getcpuclockid()` function is part of the Thread CPU-Time Clocks option and need not be provided on all implementations.

## RATIONALE

If an implementation detects use of a thread ID after the end of its lifetime, it is recommended that the function should fail and report an [ESRCH] error.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- [`clock_getcpuclockid()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/clock_getcpuclockid.html)
- [`clock_getres()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/clock_getres.html)
- [`timer_create()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/timer_create.html)

XBD [`<pthread.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/pthread.h.html), [`<time.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/time.h.html)

## CHANGE HISTORY

First released in Issue 6. Derived from IEEE Std 1003.1d-1999.

In the SYNOPSIS, the inclusion of [`<sys/types.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/sys_types.h.html) is no longer required.

### Issue 7

The `pthread_getcpuclockid()` function is marked only as part of the Thread CPU-Time Clocks option as the Threads option is now part of the Base.

Austin Group Interpretation 1003.1-2001 #142 is applied, removing the [ESRCH] error condition.

POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0275 [757] is applied.

