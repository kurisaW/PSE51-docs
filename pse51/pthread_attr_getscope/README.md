# pthread_attr_getscope, pthread_attr_setscope

## SYNOPSIS

```c
#include <pthread.h>

int pthread_attr_getscope(const pthread_attr_t *restrict attr,
                         int *restrict contentionscope);

int pthread_attr_setscope(pthread_attr_t *attr, int contentionscope);
```

## DESCRIPTION

The `pthread_attr_getscope()` and `pthread_attr_setscope()` functions, respectively, shall get and set the *contentionscope* attribute in the *attr* object.

The *contentionscope* attribute may have the values `PTHREAD_SCOPE_SYSTEM`, signifying system scheduling contention scope, or `PTHREAD_SCOPE_PROCESS`, signifying process scheduling contention scope. The symbols `PTHREAD_SCOPE_SYSTEM` and `PTHREAD_SCOPE_PROCESS` are defined in the `<pthread.h>` header.

The behavior is undefined if the value specified by the *attr* argument to `pthread_attr_getscope()` or `pthread_attr_setscope()` does not refer to an initialized thread attributes object.

## RETURN VALUE

If successful, the `pthread_attr_getscope()` and `pthread_attr_setscope()` functions shall return zero; otherwise, an error number shall be returned to indicate the error.

## ERRORS

The `pthread_attr_setscope()` function shall fail if:

- **[ENOTSUP]**
  An attempt was made to set the attribute to an unsupported value.

The `pthread_attr_setscope()` function may fail if:

- **[EINVAL]**
  The value of *contentionscope* is not valid.

These functions shall not return an error code of **[EINTR]**.

---

*The following sections are informative.*

## EXAMPLES

None.

## APPLICATION USAGE

After these attributes have been set, a thread can be created with the specified attributes using `pthread_create()`. Using these routines does not affect the current running thread.

See [2.9.4 Thread Scheduling](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_09_04) for further details on thread scheduling attributes and their default settings.

## RATIONALE

If an implementation detects that the value specified by the *attr* argument to `pthread_attr_getscope()` or `pthread_attr_setscope()` does not refer to an initialized thread attributes object, it is recommended that the function should fail and report an **[EINVAL]** error.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- [`pthread_attr_destroy()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_attr_destroy.html)
- [`pthread_attr_getinheritsched()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_attr_getinheritsched.html)
- [`pthread_attr_getschedpolicy()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_attr_getschedpolicy.html)
- [`pthread_attr_getschedparam()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_attr_getschedparam.html)
- [`pthread_create()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_create.html)

XBD [`<pthread.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/pthread.h.html), [`<sched.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/sched.h.html)

## CHANGE HISTORY

First released in Issue 5. Included for alignment with the POSIX Threads Extension.

Marked as part of the Realtime Threads Feature Group.

### Issue 6

The `pthread_attr_getscope()` and `pthread_attr_setscope()` functions are marked as part of the Threads and Thread Execution Scheduling options.

The **[ENOSYS]** error condition has been removed as stubs need not be provided if an implementation does not support the Thread Execution Scheduling option.

The **`restrict`** keyword is added to the `pthread_attr_getscope()` prototype for alignment with the ISO/IEC 9899:1999 standard.

IEEE Std 1003.1-2001/Cor 2-2004, item XSH/TC2/D6/81 is applied, adding a reference to [2.9.4 Thread Scheduling](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_09_04) in the APPLICATION USAGE section.

IEEE Std 1003.1-2001/Cor 2-2004, item XSH/TC2/D6/82 is applied, updating the ERRORS section to include optional errors for the case when *attr* refers to an uninitialized thread attribute object.

### Issue 7

The `pthread_attr_getscope()` and `pthread_attr_setscope()` functions are marked only as part of the Thread Execution Scheduling option as the Threads option is now part of the Base.

The **[EINVAL]** error for an uninitialized thread attributes object is removed; this condition results in undefined behavior.

POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0453 [314] is applied.

POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0264 [757] is applied.

