# sem_post — unlock a semaphore

## SYNOPSIS

```c
#include <semaphore.h>

int sem_post(sem_t *sem);
```

## DESCRIPTION

The `sem_post()` function shall unlock the semaphore referenced by `sem` by performing a semaphore unlock operation on that semaphore.

If the semaphore value resulting from this operation is positive, then no threads were blocked waiting for the semaphore to become unlocked; the semaphore value is simply incremented.

If the value of the semaphore resulting from this operation is zero, then one of the threads blocked waiting for the semaphore shall be allowed to return successfully from its call to `sem_wait()`. [PS] If the Process Scheduling option is supported, the thread to be unblocked shall be chosen in a manner appropriate to the scheduling policies and parameters in effect for the blocked threads. In the case of the schedulers SCHED_FIFO and SCHED_RR, the highest priority waiting thread shall be unblocked, and if there is more than one highest priority thread blocked waiting for the semaphore, then the highest priority thread that has been waiting the longest shall be unblocked. If the Process Scheduling option is not defined, the choice of a thread to unblock is unspecified.

[SS] If the Process Sporadic Server option is supported, and the scheduling policy is SCHED_SPORADIC, the semantics are as per SCHED_FIFO above.

The `sem_post()` function shall be async-signal-safe and may be invoked from a signal-catching function.

## RETURN VALUE

If successful, the `sem_post()` function shall return zero; otherwise, the function shall return -1 and set `errno` to indicate the error.

## ERRORS

The `sem_post()` function shall fail if:

- **[EOVERFLOW]** The maximum allowable value of the semaphore would be exceeded.

The `sem_post()` function may fail if:

- **[EINVAL]** The `sem` argument does not refer to a valid semaphore.

## EXAMPLES

See `sem_clockwait()`.

## APPLICATION USAGE

None.

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- `semctl()`
- `semget()`
- `semop()`
- `sem_clockwait()`
- `sem_trywait()`

XBD 4.15.2 Memory Synchronization, `<semaphore.h>`

## CHANGE HISTORY

### First released in Issue 5
Included for alignment with the POSIX Realtime Extension.

### Issue 6
- The `sem_post()` function is marked as part of the Semaphores option.
- The [ENOSYS] error condition has been removed as stubs need not be provided if an implementation does not support the Semaphores option.
- The `sem_timedwait()` function is added to the SEE ALSO section for alignment with IEEE Std 1003.1d-1999.
- SCHED_SPORADIC is added to the list of scheduling policies for which the thread that is to be unblocked is specified for alignment with IEEE Std 1003.1d-1999.
- IEEE Std 1003.1-2001/Cor 2-2004, item XSH/TC2/D6/119 is applied, updating the ERRORS section so that the [EINVAL] error becomes optional.

### Issue 7
- Austin Group Interpretation 1003.1-2001 #156 is applied.
- The `sem_post()` function is moved from the Semaphores option to the Base.
- POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0528 [37] is applied.

### Issue 8
- Austin Group Defect 315 is applied, adding the [EOVERFLOW] error.

---
