# sem_unlink - remove a named semaphore

## SYNOPSIS

```c
#include <semaphore.h>

int sem_unlink(const char *name);
```

## DESCRIPTION

The `sem_unlink()` function shall remove the semaphore named by the string `name`. If the semaphore named by `name` is currently referenced by other processes, then `sem_unlink()` shall have no effect on the state of the semaphore. If one or more processes have the semaphore open when `sem_unlink()` is called, destruction of the semaphore is postponed until all references to the semaphore have been destroyed by calls to `sem_close()`, `_exit()`, or `exec`. Calls to `sem_open()` to recreate or reconnect to the semaphore refer to a new semaphore after `sem_unlink()` is called. The `sem_unlink()` call shall not block until all references have been destroyed; it shall return immediately.

## RETURN VALUE

Upon successful completion, the `sem_unlink()` function shall return a value of 0. Otherwise, the semaphore shall not be changed and the function shall return a value of -1 and set `errno` to indicate the error.

## ERRORS

The `sem_unlink()` function shall fail if:

- **[EACCES]** - Permission is denied to unlink the named semaphore.
- **[ENOENT]** - The named semaphore does not exist.

The `sem_unlink()` function may fail if:

- **[ENAMETOOLONG]** - The length of the `name` argument exceeds {_POSIX_PATH_MAX} on systems that do not support the XSI option or exceeds {_XOPEN_PATH_MAX} on XSI systems, or has a pathname component that is longer than {_POSIX_NAME_MAX} on systems that do not support the XSI option or longer than {_XOPEN_NAME_MAX} on XSI systems. A call to `sem_unlink()` with a `name` argument that contains the same semaphore name as was previously used in a successful `sem_open()` call shall not give an [ENAMETOOLONG] error.

## EXAMPLES

None.

## APPLICATION USAGE

None.

## RATIONALE

None.

## FUTURE DIRECTIONS

A future version might require the `sem_open()` and `sem_unlink()` functions to have semantics similar to normal file system operations.

## SEE ALSO

- [`semctl()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/semctl.html)
- [`semget()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/semget.html)
- [`semop()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/semop.html)
- [`sem_close()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_close.html)
- [`sem_open()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_open.html)
- XBD [`<semaphore.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/semaphore.h.html)

## CHANGE HISTORY

### First released in Issue 5
Included for alignment with the POSIX Realtime Extension.

### Issue 6
- The `sem_unlink()` function is marked as part of the Semaphores option.
- The [ENOSYS] error condition has been removed as stubs need not be provided if an implementation does not support the Semaphores option.

### Issue 7
- Austin Group Interpretation 1003.1-2001 #077 is applied, changing [ENAMETOOLONG] from a "shall fail" to a "may fail" error.
- Austin Group Interpretation 1003.1-2001 #141 is applied, adding FUTURE DIRECTIONS.
- The `sem_unlink()` function is moved from the Semaphores option to the Base.
- POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0531 [37] is applied.

---

*The Open Group Base Specifications Issue 8*
*IEEE Std 1003.1-2024*
*Copyright © 2001-2024 The IEEE and The Open Group*