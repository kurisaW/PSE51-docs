# pthread_setschedprio

## SYNOPSIS

```c
#include <pthread.h>

int pthread_setschedprio(pthread_t thread, int prio);
```

## DESCRIPTION

The `pthread_setschedprio()` function shall set the scheduling priority for the thread whose thread ID is given by `thread` to the value given by `prio`. See Scheduling Policies for a description on how this function call affects the ordering of the thread in the thread list for its new priority.

If the `pthread_setschedprio()` function fails, the scheduling priority of the target thread shall not be changed.

## RETURN VALUE

If successful, the `pthread_setschedprio()` function shall return zero; otherwise, an error number shall be returned to indicate the error.

## ERRORS

The `pthread_setschedprio()` function may fail if:

- **[EINVAL]**
  The value of `prio` is invalid for the scheduling policy of the specified thread.

- **[EPERM]**
  The caller does not have appropriate privileges to set the scheduling priority of the specified thread.

The `pthread_setschedprio()` function shall not return an error code of [EINTR].

## EXAMPLES

None.

## APPLICATION USAGE

None.

## RATIONALE

The `pthread_setschedprio()` function provides a way for an application to temporarily raise its priority and then lower it again, without having the undesired side-effect of yielding to other threads of the same priority. This is necessary if the application is to implement its own strategies for bounding priority inversion, such as priority inheritance or priority ceilings. This capability is especially important if the implementation does not support the Thread Priority Protection or Thread Priority Inheritance options, but even if those options are supported it is needed if the application is to bound priority inheritance for other resources, such as semaphores.

The standard developers considered that while it might be preferable conceptually to solve this problem by modifying the specification of `pthread_setschedparam()`, it was too late to make such a change, as there may be implementations that would need to be changed. Therefore, this new function was introduced.

If an implementation detects use of a thread ID after the end of its lifetime, it is recommended that the function should fail and report an [ESRCH] error.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- [Scheduling Policies](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_08_04_01)
- [`pthread_getschedparam()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_getschedparam.html)
- [`<pthread.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/pthread.h.html)

## CHANGE HISTORY

First released in Issue 6. Included as a response to IEEE PASC Interpretation 1003.1 #96.

### Issue 7

The `pthread_setschedprio()` function is marked only as part of the Thread Execution Scheduling option as the Threads option is now part of the Base.

Austin Group Interpretation 1003.1-2001 #069 is applied, updating the [EPERM] error.

Austin Group Interpretation 1003.1-2001 #142 is applied, removing the [ESRCH] error condition.

POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0466 [314] is applied.

POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0296 [757] is applied.