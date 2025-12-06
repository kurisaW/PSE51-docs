# sem_destroy — destroy an unnamed semaphore

## SYNOPSIS

```c
#include <semaphore.h>

int sem_destroy(sem_t *sem);
```

## DESCRIPTION

The `sem_destroy()` function shall destroy the unnamed semaphore indicated by `sem`. If an unnamed semaphore is implemented using a file descriptor, the file descriptor shall be closed. Only a semaphore that was created using `sem_init()` can be destroyed using `sem_destroy()`; the effect of calling `sem_destroy()` with a named semaphore is undefined. The effect of subsequent use of the semaphore `sem` is undefined until `sem` is reinitialized by another call to `sem_init()`.

It is safe to destroy an initialized semaphore upon which no threads are currently blocked. The effect of destroying a semaphore upon which other threads are currently blocked is undefined.

## RETURN VALUE

Upon successful completion, a value of zero shall be returned. Otherwise, a value of -1 shall be returned and `errno` set to indicate the error.

## ERRORS

The `sem_destroy()` function may fail if:

- **[EINVAL]**
  The `sem` argument is not a valid semaphore.

- **[EBUSY]**
  There are currently processes blocked on the semaphore.

## EXAMPLES

None.

## APPLICATION USAGE

None.

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- [`semctl()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/semctl.html)
- [`semget()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/semget.html)
- [`semop()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/semop.html)
- [`sem_init()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_init.html)
- [`sem_open()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_open.html)

XBD [`<semaphore.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/semaphore.h.html)

## CHANGE HISTORY

### First released in Issue 5
Included for alignment with the POSIX Realtime Extension.

### Issue 6
- The `sem_destroy()` function is marked as part of the Semaphores option.
- The `[ENOSYS]` error condition has been removed as stubs need not be provided if an implementation does not support the Semaphores option.
- IEEE Std 1003.1-2001/Cor 2-2004, item XSH/TC2/D6/114 is applied, updating the ERRORS section so that the `[EINVAL]` error becomes optional.

### Issue 7
- The `sem_destroy()` function is moved from the Semaphores option to the Base.
- POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0524 [37] is applied.

### Issue 8
- Austin Group Defect 368 is applied, adding a requirement that if an unnamed semaphore is implemented using a file descriptor, `sem_destroy()` closes the file descriptor.

---
