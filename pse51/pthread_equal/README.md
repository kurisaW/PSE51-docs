# pthread_equal — compare thread IDs

## SYNOPSIS

```c
#include <pthread.h>

int pthread_equal(pthread_t t1, pthread_t t2);
```

## DESCRIPTION

This function shall compare the thread IDs `t1` and `t2`.

## RETURN VALUE

The `pthread_equal()` function shall return a non-zero value if `t1` and `t2` are equal; otherwise, zero shall be returned.

If either `t1` or `t2` is not a valid thread ID and is not equal to PTHREAD_NULL, the behavior is undefined.

## ERRORS

No errors are defined.

The `pthread_equal()` function shall not return an error code of [EINTR].

*The following sections are informative.*

## EXAMPLES

None.

## APPLICATION USAGE

None.

## RATIONALE

Implementations may choose to define a thread ID as a structure. This allows additional flexibility and robustness over using an `int`. For example, a thread ID could include a sequence number that allows detection of "dangling IDs" (copies of a thread ID that has been detached). Since the C language does not support comparison on structure types, the `pthread_equal()` function is provided to compare thread IDs.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- [`pthread_create()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_create.html)
- [`pthread_self()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_self.html)
- XBD [`<pthread.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/pthread.h.html)

## CHANGE HISTORY

**First released in Issue 5.** Included for alignment with the POSIX Threads Extension.

**Issue 6**
- The `pthread_equal()` function is marked as part of the Threads option.

**Issue 7**
- The `pthread_equal()` function is moved from the Threads option to the Base.

**Issue 8**
- Austin Group Defect 599 is applied, changing the RETURN VALUE section to mention PTHREAD_NULL.

---
