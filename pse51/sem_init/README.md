# sem_init — initialize an unnamed semaphore

## SYNOPSIS

```c
#include <semaphore.h>

int sem_init(sem_t *sem, int pshared, unsigned value);
```

## DESCRIPTION

The `sem_init()` function shall initialize the unnamed semaphore referred to by `sem`. The value of the initialized semaphore shall be `value`. Following a successful call to `sem_init()`, the semaphore can be used in subsequent calls to `sem_clockwait()`, `sem_destroy()`, `sem_post()`, `sem_timedwait()`, `sem_trywait()`, and `sem_wait()`. This semaphore shall remain usable until the semaphore is destroyed. An unnamed semaphore may be implemented using a file descriptor.

If the `pshared` argument has a non-zero value, then the semaphore is shared between processes; in this case, any process that can access the semaphore `sem` can use `sem` for performing `sem_clockwait()`, `sem_destroy()`, `sem_post()`, `sem_timedwait()`, `sem_trywait()`, and `sem_wait()` operations.

If the `pshared` argument is zero, then the semaphore is shared between threads of the process; any thread in this process can use `sem` for performing `sem_clockwait()`, `sem_destroy()`, `sem_post()`, `sem_timedwait()`, `sem_trywait()`, and `sem_wait()` operations.

See [2.9.9 Synchronization Object Copies and Alternative Mappings](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_09_09) for further requirements.

Attempting to initialize an already initialized semaphore results in undefined behavior.

## RETURN VALUE

Upon successful completion, the `sem_init()` function shall initialize the semaphore in `sem` and return 0. Otherwise, it shall return -1 and set `errno` to indicate the error.

## ERRORS

The `sem_init()` function shall fail if:

- **[EINVAL]**
  The `value` argument exceeds {SEM_VALUE_MAX}.

- **[ENOSPC]**
  A resource required to initialize the semaphore has been exhausted, or the limit on semaphores ({SEM_NSEMS_MAX}) has been reached.

- **[EPERM]**
  The process lacks appropriate privileges to initialize the semaphore.

The `sem_init()` function may fail if:

- **[EMFILE]**
  All file descriptors available to the process are currently open.

- **[ENFILE]**
  The maximum allowable number of files is currently open in the system.

## EXAMPLES

None.

## APPLICATION USAGE

None.

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- [`sem_clockwait()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_clockwait.html)
- [`sem_destroy()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_destroy.html)
- [`sem_post()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_post.html)
- [`sem_trywait()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_trywait.html)
- [`<semaphore.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/semaphore.h.html)

## CHANGE HISTORY

### First released in Issue 5
Included for alignment with the POSIX Realtime Extension.

### Issue 6
- The `sem_init()` function is marked as part of the Semaphores option.
- The [ENOSYS] error condition has been removed as stubs need not be provided if an implementation does not support the Semaphores option.
- The `sem_timedwait()` function is added to the SEE ALSO section for alignment with IEEE Std 1003.1d-1999.
- IEEE Std 1003.1-2001/Cor 2-2004, item XSH/TC2/D6/116 is applied, updating the DESCRIPTION to add the `sem_timedwait()` function for alignment with IEEE Std 1003.1d-1999.

### Issue 7
- SD5-XSH-ERN-176 is applied.
- The `sem_init()` function is moved from the Semaphores option to the Base.
- POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0526 [37] is applied.
- POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0318 [972] is applied.

### Issue 8
- Austin Group Defect 368 is applied, adding a statement that an unnamed semaphore may be implemented using a file descriptor and adding the [EMFILE] and [ENFILE] errors.
- Austin Group Defect 1216 is applied, adding `sem_clockwait()`.

---

*The Open Group Base Specifications Issue 8*
*IEEE Std 1003.1-2024*
*Copyright © 2001-2024 The IEEE and The Open Group*