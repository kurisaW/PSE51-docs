# pthread_mutex_getprioceiling, pthread_mutex_setprioceiling

## SYNOPSIS

```c
[RPP|TPP]
#include <pthread.h>

int pthread_mutex_getprioceiling(const pthread_mutex_t *restrict mutex,
                                int *restrict prioceiling);

int pthread_mutex_setprioceiling(pthread_mutex_t *restrict mutex,
                                int prioceiling, int *restrict old_ceiling);
```

## DESCRIPTION

The `pthread_mutex_getprioceiling()` function shall return the current priority ceiling of the mutex.

The `pthread_mutex_setprioceiling()` function shall attempt to lock the mutex as if by a call to `pthread_mutex_lock()`, except that the process of locking the mutex need not adhere to the priority protect protocol. On acquiring the mutex it shall change the mutex's priority ceiling and then release the mutex as if by a call to `pthread_mutex_unlock()`. When the change is successful, the previous value of the priority ceiling shall be returned in `old_ceiling`.

If the `pthread_mutex_setprioceiling()` function fails, the mutex priority ceiling shall not be changed.

## RETURN VALUE

If successful, the `pthread_mutex_getprioceiling()` and `pthread_mutex_setprioceiling()` functions shall return zero; otherwise, an error number shall be returned to indicate the error.

## ERRORS

These functions shall fail if:

- **[EINVAL]**
  The protocol attribute of `mutex` is PTHREAD_PRIO_NONE.

- **[EPERM]**
  The implementation requires appropriate privileges to perform the operation and the caller does not have appropriate privileges.

The `pthread_mutex_setprioceiling()` function shall fail if:

- **[EAGAIN]**
  The mutex could not be acquired because the maximum number of recursive locks for `mutex` has been exceeded.

- **[EAGAIN]**
  The mutex is a robust mutex and the system resources available for robust mutexes owned would be exceeded.

- **[EDEADLK]**
  The mutex type is PTHREAD_MUTEX_ERRORCHECK and the current thread already owns the mutex.

- **[EINVAL]**
  The mutex was created with the protocol attribute having the value PTHREAD_PRIO_PROTECT and the calling thread's priority is higher than the mutex's current priority ceiling, and the implementation adheres to the priority protect protocol in the process of locking the mutex.

- **[ENOTRECOVERABLE]**
  The mutex is a robust mutex and the state protected by the mutex is not recoverable.

- **[EOWNERDEAD]**
  The mutex is a robust mutex and the process containing the previous owning thread terminated while holding the mutex lock. The mutex lock shall be acquired by the calling thread and it is up to the new owner to make the state consistent (see `pthread_mutex_lock()`).

The `pthread_mutex_setprioceiling()` function may fail if:

- **[EDEADLK]**
  A deadlock condition was detected.

- **[EINVAL]**
  The priority requested by `prioceiling` is out of range.

- **[EOWNERDEAD]**
  The mutex is a robust mutex and the previous owning thread terminated while holding the mutex lock. The mutex lock shall be acquired by the calling thread and it is up to the new owner to make the state consistent (see `pthread_mutex_lock()`).

These functions shall not return an error code of [EINTR].

---

## EXAMPLES

None.

## APPLICATION USAGE

None.

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- `pthread_mutex_clocklock()`
- `pthread_mutex_destroy()`
- `pthread_mutex_lock()`
- XBD `<pthread.h>`

## CHANGE HISTORY

### First released in Issue 5
Included for alignment with the POSIX Threads Extension.
Marked as part of the Realtime Threads Feature Group.

### Issue 6
The `pthread_mutex_getprioceiling()` and `pthread_mutex_setprioceiling()` functions are marked as part of the Threads and Thread Priority Protection options.
The [ENOSYS] error conditions have been removed.
The `pthread_mutex_timedlock()` function is added to the SEE ALSO section for alignment with IEEE Std 1003.1d-1999.
The **restrict** keyword is added to the `pthread_mutex_getprioceiling()` and `pthread_mutex_setprioceiling()` prototypes for alignment with the ISO/IEC 9899:1999 standard.

### Issue 7
SD5-XSH-ERN-39 is applied.
Austin Group Interpretation 1003.1-2001 #052 is applied, adding [EDEADLK] as a "may fail" error.
SD5-XSH-ERN-158 is applied, updating the ERRORS section to include a "shall fail" error case for when the protocol attribute of `mutex` is PTHREAD_PRIO_NONE.
The `pthread_mutex_getprioceiling()` and `pthread_mutex_setprioceiling()` functions are moved from the Threads option to require support of either the Robust Mutex Priority Protection option or the Non-Robust Mutex Priority Protection option.
The DESCRIPTION and ERRORS sections are updated to account properly for all of the various mutex types.

### Issue 8
Austin Group Defect 354 is applied, adding the [EAGAIN] error for exceeding system resources available for robust mutexes owned.