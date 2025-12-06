# sem_close — close a named semaphore

## SYNOPSIS

```c
#include <semaphore.h>

int sem_close(sem_t *sem);
```

## DESCRIPTION

The `sem_close()` function shall indicate that the calling process is finished using the named semaphore indicated by `sem`. The effects of calling `sem_close()` for an unnamed semaphore (one created by `sem_init()`) are undefined. The `sem_close()` function shall deallocate (that is, make available for reuse by a subsequent `sem_open()` by this process) any system resources allocated by the system for use by this process for this semaphore. If the semaphore indicated by `sem` is implemented using a file descriptor, the file descriptor shall be closed. The effect of subsequent use of the semaphore indicated by `sem` by this process is undefined. If any threads in the calling process are currently blocked on the semaphore, the behavior is undefined. If the semaphore has not been removed with a successful call to `sem_unlink()`, then `sem_close()` has no effect on the state of the semaphore. If the `sem_unlink()` function has been successfully invoked for `name` after the most recent call to `sem_open()` with O_CREAT for this semaphore, then when all processes that have opened the semaphore close all semaphore handles to it, the semaphore is no longer accessible.

## RETURN VALUE

Upon successful completion, a value of zero shall be returned. Otherwise, a value of -1 shall be returned and `errno` set to indicate the error.

## ERRORS

The `sem_close()` function may fail if:

- **[EINVAL]**

  The `sem` argument is not a valid semaphore descriptor.

## EXAMPLES

None.

## APPLICATION USAGE

None.

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

[`semctl()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/semctl.html), [`semget()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/semget.html), [`semop()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/semop.html), [`sem_init()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_init.html), [`sem_open()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_open.html), [`sem_unlink()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_unlink.html)

XBD [`<semaphore.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/semaphore.h.html)

## CHANGE HISTORY

### Issue 6

The `sem_close()` function is marked as part of the Semaphores option.

The [ENOSYS] error condition has been removed as stubs need not be provided if an implementation does not support the Semaphores option.

IEEE Std 1003.1-2001/Cor 2-2004, item XSH/TC2/D6/113 is applied, updating the ERRORS section so that the [EINVAL] error becomes optional.

### Issue 7

The `sem_close()` function is moved from the Semaphores option to the Base.

POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0523 [37] is applied.

POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0317 [870] is applied.

### Issue 8

Austin Group Defect 368 is applied, adding a requirement that if `sem` is implemented using a file descriptor, `sem_close()` closes the file descriptor.

Austin Group Defect 1324 is applied, clarifying the circumstances under which an unlinked semaphore is no longer accessible.

---
