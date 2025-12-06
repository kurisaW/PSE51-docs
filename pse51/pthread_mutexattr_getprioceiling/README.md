# pthread_mutexattr_getprioceiling, pthread_mutexattr_setprioceiling

The Open Group Base Specifications Issue 8
IEEE Std 1003.1-2024
Copyright © 2001-2024 The IEEE and The Open Group

## NAME

pthread_mutexattr_getprioceiling, pthread_mutexattr_setprioceiling — get and set the prioceiling attribute of the mutex attributes object (**REALTIME THREADS**)

## SYNOPSIS

```c
#include <pthread.h>

int pthread_mutexattr_getprioceiling(const pthread_mutexattr_t *restrict attr,
                                   int *restrict prioceiling);

int pthread_mutexattr_setprioceiling(pthread_mutexattr_t *attr,
                                   int prioceiling);
```

## DESCRIPTION

The `pthread_mutexattr_getprioceiling()` and `pthread_mutexattr_setprioceiling()` functions, respectively, shall get and set the priority ceiling attribute of a mutex attributes object pointed to by `attr` which was previously created by the function `pthread_mutexattr_init()`.

The `prioceiling` attribute contains the priority ceiling of initialized mutexes. The values of `prioceiling` are within the maximum range of priorities defined by SCHED_FIFO.

The `prioceiling` attribute defines the priority ceiling of initialized mutexes, which is the minimum priority level at which the critical section guarded by the mutex is executed. In order to avoid priority inversion, the priority ceiling of the mutex shall be set to a priority higher than or equal to the highest priority of all the threads that may lock that mutex. The values of `prioceiling` are within the maximum range of priorities defined under the SCHED_FIFO scheduling policy.

The behavior is undefined if the value specified by the `attr` argument to `pthread_mutexattr_getprioceiling()` or `pthread_mutexattr_setprioceiling()` does not refer to an initialized mutex attributes object.

## RETURN VALUE

Upon successful completion, the `pthread_mutexattr_getprioceiling()` and `pthread_mutexattr_setprioceiling()` functions shall return zero; otherwise, an error number shall be returned to indicate the error.

## ERRORS

These functions may fail if:

- **[EINVAL]**
  The value specified by `prioceiling` is invalid.

- **[EPERM]**
  The caller does not have the privilege to perform the operation.

These functions shall not return an error code of [EINTR].

*The following sections are informative.*

## EXAMPLES

None.

## APPLICATION USAGE

None.

## RATIONALE

If an implementation detects that the value specified by the `attr` argument to `pthread_mutexattr_getprioceiling()` or `pthread_mutexattr_setprioceiling()` does not refer to an initialized mutex attributes object, it is recommended that the function should fail and report an [EINVAL] error.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- `pthread_cond_destroy()`
- `pthread_create()`
- `pthread_mutex_destroy()`
- `<pthread.h>`

## CHANGE HISTORY

First released in Issue 5. Included for alignment with the POSIX Threads Extension.

Marked as part of the Realtime Threads Feature Group.

### Issue 6

The `pthread_mutexattr_getprioceiling()` and `pthread_mutexattr_setprioceiling()` functions are marked as part of the Threads and Thread Priority Protection options.

The [ENOSYS] error condition has been removed as stubs need not be provided if an implementation does not support the Thread Priority Protection option.

The [ENOTSUP] error condition has been removed since these functions do not have a `protocol` argument.

The `restrict` keyword is added to the `pthread_mutexattr_getprioceiling()` prototype for alignment with the ISO/IEC 9899:1999 standard.

### Issue 7

The `pthread_mutexattr_getprioceiling()` and `pthread_mutexattr_setprioceiling()` functions are moved from the Threads option to require support of either the Robust Mutex Priority Protection option or the Non-Robust Mutex Priority Protection option.

The [EINVAL] error for an uninitialized mutex attributes object is removed; this condition results in undefined behavior.

---

