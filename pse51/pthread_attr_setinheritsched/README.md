# pthread_attr_getinheritsched, pthread_attr_setinheritsched — get and set the inheritsched attribute (REALTIME THREADS)

## SYNOPSIS

```c
#include <pthread.h>

int pthread_attr_getinheritsched(const pthread_attr_t *restrict attr,
                                int *restrict inheritsched);

int pthread_attr_setinheritsched(pthread_attr_t *attr,
                                int inheritsched);
```

## DESCRIPTION

The `pthread_attr_getinheritsched()` and `pthread_attr_setinheritsched()` functions, respectively, shall get and set the `inheritsched` attribute in the `attr` argument.

When the attributes objects are used by `pthread_create()`, the `inheritsched` attribute determines how the other scheduling attributes of the created thread shall be set.

The supported values of `inheritsched` shall be:

- **PTHREAD_INHERIT_SCHED**
  Specifies that the thread scheduling attributes shall be inherited from the creating thread, and the scheduling attributes in this `attr` argument shall be ignored.

- **PTHREAD_EXPLICIT_SCHED**
  Specifies that the thread scheduling attributes shall be set to the corresponding values from this attributes object.

The symbols PTHREAD_INHERIT_SCHED and PTHREAD_EXPLICIT_SCHED are defined in the `<pthread.h>` header.

The following thread scheduling attributes defined by POSIX.1-2024 are affected by the `inheritsched` attribute: scheduling policy (`schedpolicy`), scheduling parameters (`schedparam`), and scheduling contention scope (`contentionscope`).

The behavior is undefined if the value specified by the `attr` argument to `pthread_attr_getinheritsched()` or `pthread_attr_setinheritsched()` does not refer to an initialized thread attributes object.

## RETURN VALUE

If successful, the `pthread_attr_getinheritsched()` and `pthread_attr_setinheritsched()` functions shall return zero; otherwise, an error number shall be returned to indicate the error.

## ERRORS

The `pthread_attr_setinheritsched()` function shall fail if:

- **[ENOTSUP]**
  An attempt was made to set the attribute to an unsupported value.

The `pthread_attr_setinheritsched()` function may fail if:

- **[EINVAL]**
  The value of `inheritsched` is not valid.

These functions shall not return an error code of `[EINTR]`.

## EXAMPLES

None.

## APPLICATION USAGE

After these attributes have been set, a thread can be created with the specified attributes using `pthread_create()`. Using these routines does not affect the current running thread.

See [2.9.4 Thread Scheduling](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_09_04) for further details on thread scheduling attributes and their default settings.

## RATIONALE

If an implementation detects that the value specified by the `attr` argument to `pthread_attr_getinheritsched()` or `pthread_attr_setinheritsched()` does not refer to an initialized thread attributes object, it is recommended that the function should fail and report an `[EINVAL]` error.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- `pthread_attr_destroy()`
- `pthread_attr_getscope()`
- `pthread_attr_getschedpolicy()`
- `pthread_attr_getschedparam()`
- `pthread_create()`
- `<pthread.h>`
- `<sched.h>`

## CHANGE HISTORY

First released in Issue 5. Included for alignment with the POSIX Threads Extension.

Marked as part of the Realtime Threads Feature Group.

### Issue 6

The `pthread_attr_getinheritsched()` and `pthread_attr_setinheritsched()` functions are marked as part of the Threads and Thread Execution Scheduling options.

The `[ENOSYS]` error condition has been removed as stubs need not be provided if an implementation does not support the Thread Execution Scheduling option.

The **restrict** keyword is added to the `pthread_attr_getinheritsched()` prototype for alignment with the ISO/IEC 9899:1999 standard.

IEEE Std 1003.1-2001/Cor 2-2004, item XSH/TC2/D6/75 is applied, clarifying the values of `inheritsched` in the DESCRIPTION and adding two optional `[EINVAL]` errors to the ERRORS section for checking when `attr` refers to an uninitialized thread attribute object.

IEEE Std 1003.1-2001/Cor 2-2004, item XSH/TC2/D6/77 is applied, adding a reference to [2.9.4 Thread Scheduling](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_09_04) in the APPLICATION USAGE section.

### Issue 7

The `pthread_attr_getinheritsched()` and `pthread_attr_setinheritsched()` functions are marked only as part of the Thread Execution Scheduling option as the Threads option is now part of the Base.

The `[EINVAL]` error for an uninitialized thread attributes object is removed; this condition results in undefined behavior.

POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0450 [314] is applied.

POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0262 [757] is applied.