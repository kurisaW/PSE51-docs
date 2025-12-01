# sem_trywait, sem_wait — lock a semaphore

## SYNOPSIS

```c
#include <semaphore.h>

int sem_trywait(sem_t *sem);
int sem_wait(sem_t *sem);
```

## DESCRIPTION

The `sem_trywait()` function shall lock the semaphore referenced by `sem` only if the semaphore is currently not locked; that is, if the semaphore value is currently positive. Otherwise, it shall not lock the semaphore.

The `sem_wait()` function shall lock the semaphore referenced by `sem` by performing a semaphore lock operation on that semaphore. If the semaphore value is currently zero, then the calling thread shall not return from the call to `sem_wait()` until it either locks the semaphore or the call is interrupted by a signal.

Upon successful return, the state of the semaphore shall be locked and shall remain locked until the `sem_post()` function is executed and returns successfully.

The `sem_wait()` function is interruptible by the delivery of a signal.

## RETURN VALUE

The `sem_trywait()` and `sem_wait()` functions shall return zero if the calling process successfully performed the semaphore lock operation on the semaphore designated by `sem`. If the call was unsuccessful, the state of the semaphore shall be unchanged, and the function shall return a value of -1 and set `errno` to indicate the error.

## ERRORS

The `sem_trywait()` function shall fail if:

- **[EAGAIN]**
  The semaphore was already locked, so it cannot be immediately locked by the `sem_trywait()` operation.

The `sem_trywait()` and `sem_wait()` functions may fail if:

- **[EDEADLK]**
  A deadlock condition was detected.

- **[EINTR]**
  A signal interrupted this function.

- **[EINVAL]**
  The `sem` argument does not refer to a valid semaphore.

---

## APPLICATION USAGE

Applications using these functions may be subject to priority inversion, as discussed in XBD 3.275 Priority Inversion.

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- [`semctl()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/semctl.html)
- [`semget()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/semget.html)
- [`semop()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/semop.html)
- [`sem_clockwait()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_clockwait.html)
- [`sem_post()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_post.html)

XBD 3.275 Priority Inversion, 4.15.2 Memory Synchronization, `<semaphore.h>`

## CHANGE HISTORY

### First released in Issue 5
Included for alignment with the POSIX Realtime Extension.

### Issue 6
- The `sem_trywait()` and `sem_wait()` functions are marked as part of the Semaphores option.
- The [ENOSYS] error condition has been removed as stubs need not be provided if an implementation does not support the Semaphores option.
- The `sem_timedwait()` function is added to the SEE ALSO section for alignment with IEEE Std 1003.1d-1999.
- IEEE Std 1003.1-2001/Cor 2-2004, item XSH/TC2/D6/121 is applied, updating the ERRORS section so that the [EINVAL] error becomes optional.

### Issue 7
- SD5-XSH-ERN-54 is applied, removing the `sem_wait()` function from the "shall fail" error cases.
- The `sem_trywait()` and `sem_wait()` functions are moved from the Semaphores option to the Base.
- POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0530 [37] is applied.

---

*The Open Group Base Specifications Issue 8*
*IEEE Std 1003.1-2024*
*Copyright © 2001-2024 The IEEE and The Open Group*