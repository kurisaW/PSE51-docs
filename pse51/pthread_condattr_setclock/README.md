# pthread_condattr_getclock, pthread_condattr_setclock

## SYNOPSIS

```c
#include <pthread.h>

int pthread_condattr_getclock(const pthread_condattr_t *restrict attr,
                             clockid_t *restrict clock_id);

int pthread_condattr_setclock(pthread_condattr_t *attr,
                             clockid_t clock_id);
```

## DESCRIPTION

The `pthread_condattr_getclock()` function shall obtain the value of the *clock* attribute from the attributes object referenced by *attr*.

The `pthread_condattr_setclock()` function shall set the *clock* attribute in an initialized attributes object referenced by *attr*. If `pthread_condattr_setclock()` is called with a *clock_id* argument that refers to a CPU-time clock, the call shall fail.

The *clock* attribute is the clock ID of the clock that shall be used to measure the timeout service of `pthread_cond_timedwait()`. The default value of the *clock* attribute shall refer to the system clock. The *clock* attribute shall have no effect on the `pthread_cond_clockwait()` function.

The behavior is undefined if the value specified by the *attr* argument to `pthread_condattr_getclock()` or `pthread_condattr_setclock()` does not refer to an initialized condition variable attributes object.

## RETURN VALUE

If successful, the `pthread_condattr_getclock()` function shall return zero and store the value of the clock attribute of *attr* into the object referenced by the *clock_id* argument. Otherwise, an error number shall be returned to indicate the error.

If successful, the `pthread_condattr_setclock()` function shall return zero; otherwise, an error number shall be returned to indicate the error.

## ERRORS

The `pthread_condattr_setclock()` function may fail if:

- **[EINVAL]**
  - The value specified by *clock_id* does not refer to a known clock, or is a CPU-time clock.

These functions shall not return an error code of **[EINTR]**.

---

*The following sections are informative.*

## EXAMPLES

None.

## APPLICATION USAGE

None.

## RATIONALE

If an implementation detects that the value specified by the *attr* argument to `pthread_condattr_getclock()` or `pthread_condattr_setclock()` does not refer to an initialized condition variable attributes object, it is recommended that the function should fail and report an **[EINVAL]** error.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- [`pthread_cond_clockwait()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_cond_clockwait.html)
- [`pthread_cond_destroy()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_cond_destroy.html)
- [`pthread_condattr_destroy()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_condattr_destroy.html)
- [`pthread_condattr_getpshared()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_condattr_getpshared.html)
- [`pthread_create()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_create.html)
- [`pthread_mutex_destroy()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_mutex_destroy.html)

XBD [`<pthread.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/pthread.h.html)

## CHANGE HISTORY

First released in Issue 6. Derived from IEEE Std 1003.1j-2000.

### Issue 7

The `pthread_condattr_getclock()` and `pthread_condattr_setclock()` functions are moved from the Clock Selection option to the Base.

The **[EINVAL]** error for an uninitialized condition variable attributes object is removed; this condition results in undefined behavior.

### Issue 8

Austin Group Defect 1216 is applied, adding [`pthread_cond_clockwait()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_cond_clockwait.html).

