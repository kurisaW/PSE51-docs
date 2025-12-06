# pthread_cond_broadcast, pthread_cond_signal — broadcast or signal a condition

## SYNOPSIS

```c
#include <pthread.h>

int pthread_cond_broadcast(pthread_cond_t *cond);
int pthread_cond_signal(pthread_cond_t *cond);
```

## DESCRIPTION

These functions shall unblock threads blocked on a condition variable.

The `pthread_cond_broadcast()` function shall, as a single atomic operation, determine which threads, if any, are blocked on the specified condition variable `cond` and unblock all of these threads.

The `pthread_cond_signal()` function shall, as a single atomic operation, determine which threads, if any, are blocked on the specified condition variable `cond` and unblock at least one of these threads.

If more than one thread is blocked on a condition variable, the scheduling policy shall determine the order in which threads are unblocked. When each thread unblocked as a result of a `pthread_cond_broadcast()` or `pthread_cond_signal()` returns from its call to `pthread_cond_clockwait()`, `pthread_cond_timedwait()`, or `pthread_cond_wait()`, the thread shall own the mutex with which it called `pthread_cond_clockwait()`, `pthread_cond_timedwait()`, or `pthread_cond_wait()`. The thread(s) that are unblocked shall contend for the mutex according to the scheduling policy (if applicable), and as if each had called `pthread_mutex_lock()`.

The `pthread_cond_broadcast()` or `pthread_cond_signal()` functions may be called by a thread whether or not it currently owns the mutex that threads calling `pthread_cond_clockwait()`, `pthread_cond_timedwait()`, or `pthread_cond_wait()` have associated with the condition variable during their waits; however, if predictable scheduling behavior is required, then that mutex shall be locked by the thread calling `pthread_cond_broadcast()` or `pthread_cond_signal()`.

The `pthread_cond_broadcast()` and `pthread_cond_signal()` functions shall have no effect if they determine that there are no threads blocked on `cond`.

The behavior is undefined if the value specified by the `cond` argument to `pthread_cond_broadcast()` or `pthread_cond_signal()` does not refer to an initialized condition variable.

## RETURN VALUE

If successful, the `pthread_cond_broadcast()` and `pthread_cond_signal()` functions shall return zero; otherwise, an error number shall be returned to indicate the error.

## ERRORS

These functions shall not return an error code of `[EINTR]`.

## EXAMPLES

None.

## APPLICATION USAGE

The `pthread_cond_broadcast()` function is used whenever the shared-variable state has been changed in a way that more than one thread can proceed with its task. Consider a single producer/multiple consumer problem, where the producer can insert multiple items on a list that is accessed one item at a time by the consumers. By calling the `pthread_cond_broadcast()` function, the producer would notify all consumers that might be waiting, and thereby the application would receive more throughput on a multi-processor. In addition, `pthread_cond_broadcast()` makes it easier to implement a read-write lock. The `pthread_cond_broadcast()` function is needed in order to wake up all waiting readers when a writer releases its lock. Finally, the two-phase commit algorithm can use this broadcast function to notify all clients of an impending transaction commit.

It is not safe to use the `pthread_cond_signal()` function in a signal handler that is invoked asynchronously. Even if it were safe, there would still be a race between the test of the Boolean `pthread_cond_wait()` that could not be efficiently eliminated.

Mutexes and condition variables are thus not suitable for releasing a waiting thread by signaling from code running in a signal handler.

## RATIONALE

If an implementation detects that the value specified by the `cond` argument to `pthread_cond_broadcast()` or `pthread_cond_signal()` does not refer to an initialized condition variable, it is recommended that the function should fail and report an `[EINVAL]` error.

### Multiple Awakenings by Condition Signal

On a multi-processor, it may be impossible for an implementation of `pthread_cond_signal()` to avoid the unblocking of more than one thread blocked on a condition variable. For example, consider the following partial implementation of `pthread_cond_wait()` and `pthread_cond_signal()`, executed by two threads in the order given. One thread is trying to wait on the condition variable, another is concurrently executing `pthread_cond_signal()`, while a third thread is already waiting.

```c
pthread_cond_wait(mutex, cond):
    value = cond->value;             /* 1 */
    pthread_mutex_unlock(mutex);     /* 2 */
    pthread_mutex_lock(cond->mutex); /* 10 */
    if (value == cond->value) {      /* 11 */
        me->next_cond = cond->waiter;
        cond->waiter = me;
        pthread_mutex_unlock(cond->mutex);
        unable_to_run(me);
    } else
        pthread_mutex_unlock(cond->mutex); /* 12 */
    pthread_mutex_lock(mutex);       /* 13 */

pthread_cond_signal(cond):
    pthread_mutex_lock(cond->mutex); /* 3 */
    cond->value++;                   /* 4 */
    if (cond->waiter) {              /* 5 */
        sleeper = cond->waiter;      /* 6 */
        cond->waiter = sleeper->next_cond; /* 7 */
        able_to_run(sleeper);        /* 8 */
    }
    pthread_mutex_unlock(cond->mutex); /* 9 */
```

The effect is that more than one thread can return from its call to `pthread_cond_clockwait()`, `pthread_cond_timedwait()`, or `pthread_cond_wait()` as a result of one call to `pthread_cond_signal()`. This effect is called "spurious wakeup". Note that the situation is self-correcting in that the number of threads that are so awakened is finite; for example, the next thread to call `pthread_cond_wait()` after the sequence of events above blocks.

While this problem could be resolved, the loss of efficiency for a fringe condition that occurs only rarely is unacceptable, especially given that one has to check the predicate associated with a condition variable anyway. Correcting this problem would unnecessarily reduce the degree of concurrency in this basic building block for all higher-level synchronization operations.

An added benefit of allowing spurious wakeups is that applications are forced to code a predicate-testing-loop around the condition wait. This also makes the application tolerate superfluous condition broadcasts or signals on the same condition variable that may be coded in some other part of the application. The resulting applications are thus more robust. Therefore, POSIX.1-2024 explicitly documents that spurious wakeups may occur.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- `pthread_cond_clockwait()`
- `pthread_cond_destroy()`

XBD [4.15.2 Memory Synchronization](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/V1_chap04.html#tag_04_15_02), [<pthread.h>](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/pthread.h.html)

## CHANGE HISTORY

### First released in Issue 5.
Included for alignment with the POSIX Threads Extension.

### Issue 6
The `pthread_cond_broadcast()` and `pthread_cond_signal()` functions are marked as part of the Threads option.
The APPLICATION USAGE section is added.

### Issue 7
The `pthread_cond_broadcast()` and `pthread_cond_signal()` functions are moved from the Threads option to the Base.
The `[EINVAL]` error for an uninitialized condition variable is removed; this condition results in undefined behavior.

### Issue 8
Austin Group Defect 609 is applied, adding atomicity requirements.
Austin Group Defect 1216 is applied, adding `pthread_cond_clockwait()`.

---
