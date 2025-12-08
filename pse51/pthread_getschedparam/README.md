# pthread_getschedparam, pthread_setschedparam — dynamic thread scheduling parameters access (REALTIME THREADS)

## SYNOPSIS

```c
#include <pthread.h>

int pthread_getschedparam(pthread_t thread,
                         int *restrict policy,
                         struct sched_param *restrict param);

int pthread_setschedparam(pthread_t thread,
                         int policy,
                         const struct sched_param *param);
```

## DESCRIPTION

The `pthread_getschedparam()` and `pthread_setschedparam()` functions shall, respectively, get and set the scheduling policy and parameters of individual threads within a multi-threaded process to be retrieved and set. For SCHED_FIFO and SCHED_RR, the only required member of the `sched_param` structure is the priority `sched_priority`. For SCHED_OTHER, the affected scheduling parameters are implementation-defined.

The `pthread_getschedparam()` function shall retrieve the scheduling policy and scheduling parameters for the thread whose thread ID is given by `thread` and shall store those values in `policy` and `param`, respectively. The priority value returned from `pthread_getschedparam()` shall be the value specified by the most recent `pthread_setschedparam()`, `pthread_setschedprio()`, or `pthread_create()` call affecting the target thread. It shall not reflect any temporary adjustments to its priority as a result of any priority inheritance or ceiling functions. The `pthread_setschedparam()` function shall set the scheduling policy and associated scheduling parameters for the thread whose thread ID is given by `thread` to the policy and associated parameters provided in `policy` and `param`, respectively.

The `policy` parameter may have the value SCHED_OTHER, SCHED_FIFO, or SCHED_RR. The scheduling parameters for the SCHED_OTHER policy are implementation-defined. The SCHED_FIFO and SCHED_RR policies shall have a single scheduling parameter, `priority`.

If `_POSIX_THREAD_SPORADIC_SERVER` is defined, then the `policy` argument may have the value SCHED_SPORADIC, with the exception for the `pthread_setschedparam()` function that if the scheduling policy was not SCHED_SPORADIC at the time of the call, it is implementation-defined whether the function is supported; in other words, the implementation need not allow the application to dynamically change the scheduling policy to SCHED_SPORADIC. The sporadic server scheduling policy has the associated parameters `sched_ss_low_priority`, `sched_ss_repl_period`, `sched_ss_init_budget`, `sched_priority`, and `sched_ss_max_repl`. The specified `sched_ss_repl_period` shall be greater than or equal to the specified `sched_ss_init_budget` for the function to succeed; if it is not, then the function shall fail. The value of `sched_ss_max_repl` shall be within the inclusive range [1,{SS_REPL_MAX}] for the function to succeed; if not, the function shall fail. It is unspecified whether the `sched_ss_repl_period` and `sched_ss_init_budget` values are stored as provided by this function or are rounded to align with the resolution of the clock being used.

If the `pthread_setschedparam()` function fails, the scheduling parameters shall not be changed for the target thread.

## RETURN VALUE

If successful, the `pthread_getschedparam()` and `pthread_setschedparam()` functions shall return zero; otherwise, an error number shall be returned to indicate the error.

## ERRORS

The `pthread_setschedparam()` function shall fail if:

- **ENOTSUP**: An attempt was made to set the policy or scheduling parameters to an unsupported value.
- **ENOTSUP**: An attempt was made to dynamically change the scheduling policy to SCHED_SPORADIC, and the implementation does not support this change.

The `pthread_setschedparam()` function may fail if:

- **EINVAL**: The value specified by `policy` or one of the scheduling parameters associated with the scheduling policy `policy` is invalid.
- **EPERM**: The caller does not have appropriate privileges to set either the scheduling parameters or the scheduling policy of the specified thread.
- **EPERM**: The implementation does not allow the application to modify one of the parameters to the value specified.

These functions shall not return an error code of [EINTR].

## EXAMPLES

None.

## APPLICATION USAGE

None.

## RATIONALE

If an implementation detects use of a thread ID after the end of its lifetime, it is recommended that the function should fail and report an [ESRCH] error.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- [`pthread_setschedprio()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_setschedprio.html)
- [`sched_getparam()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sched_getparam.html)
- [`sched_getscheduler()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sched_getscheduler.html)
- XBD [`<pthread.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/pthread.h.html)
- XBD [`<sched.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/sched.h.html)

## CHANGE HISTORY

### First released in Issue 5
Included for alignment with the POSIX Threads Extension.

### Issue 6
- The `pthread_getschedparam()` and `pthread_setschedparam()` functions are marked as part of the Threads and Thread Execution Scheduling options.
- The [ENOSYS] error condition has been removed as stubs need not be provided if an implementation does not support the Thread Execution Scheduling option.
- The Open Group Corrigendum U026/2 is applied, correcting the prototype for the `pthread_setschedparam()` function so that its second argument is of type `int`.
- The SCHED_SPORADIC scheduling policy is added for alignment with IEEE Std 1003.1d-1999.
- The `restrict` keyword is added to the `pthread_getschedparam()` prototype for alignment with the ISO/IEC 9899:1999 standard.
- The Open Group Corrigendum U047/1 is applied.
- IEEE PASC Interpretation 1003.1 #96 is applied, noting that priority values can also be set by a call to the [`pthread_setschedprio()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_setschedprio.html) function.

### Issue 7
- The `pthread_getschedparam()` and `pthread_setschedparam()` functions are marked only as part of the Thread Execution Scheduling option as the Threads option is now part of the Base.
- Austin Group Interpretation 1003.1-2001 #119 is applied, clarifying the accuracy requirements for the `sched_ss_repl_period` and `sched_ss_init_budget` values.
- Austin Group Interpretation 1003.1-2001 #142 is applied, removing the [ESRCH] error condition.
- POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0459 [314] is applied.
- POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0276 [757] is applied.

---
