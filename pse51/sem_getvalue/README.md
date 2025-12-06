# sem_getvalue

## NAME

sem_getvalue — get the value of a semaphore

## SYNOPSIS

```c
#include <semaphore.h>

int sem_getvalue(sem_t *restrict sem, int *restrict sval);
```

## DESCRIPTION

The `sem_getvalue()` function shall update the location referenced by the `sval` argument to have the value of the semaphore referenced by `sem` without affecting the state of the semaphore. The updated value represents an actual semaphore value that occurred at some unspecified time during the call, but it need not be the actual value of the semaphore when it is returned to the calling process.

If `sem` is locked, then the object to which `sval` points shall either be set to zero or to a negative number whose absolute value represents the number of processes waiting for the semaphore at some unspecified time during the call.

## RETURN VALUE

Upon successful completion, the `sem_getvalue()` function shall return a value of zero. Otherwise, it shall return a value of -1 and set `errno` to indicate the error.

## ERRORS

The `sem_getvalue()` function may fail if:

- **[EINVAL]** - The `sem` argument does not refer to a valid semaphore.

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

- [`semctl()`](semctl.html)
- [`semget()`](semget.html)
- [`semop()`](semop.html)
- [`sem_clockwait()`](sem_clockwait.html)
- [`sem_post()`](sem_post.html)
- [`sem_trywait()`](sem_trywait.html)

XBD [`<semaphore.h>`](../basedefs/semaphore.h.html)

## CHANGE HISTORY

### First released in Issue 5.
Included for alignment with the POSIX Realtime Extension.

### Issue 6

The `sem_getvalue()` function is marked as part of the Semaphores option.

The [ENOSYS] error condition has been removed as stubs need not be provided if an implementation does not support the Semaphores option.

The [`sem_timedwait()`](sem_timedwait.html) function is added to the SEE ALSO section for alignment with IEEE Std 1003.1d-1999.

The **restrict** keyword is added to the `sem_getvalue()` prototype for alignment with the ISO/IEC 9899:1999 standard.

IEEE Std 1003.1-2001/Cor 1-2002, item XSH/TC1/D6/54 is applied.

IEEE Std 1003.1-2001/Cor 2-2004, item XSH/TC2/D6/115 is applied, updating the ERRORS section so that the [EINVAL] error becomes optional.

### Issue 7

The `sem_getvalue()` function is moved from the Semaphores option to the Base.

POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0525 [37] is applied.

---

