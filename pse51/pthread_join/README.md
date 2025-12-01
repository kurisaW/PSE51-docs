# pthread_join — wait for thread termination

## SYNOPSIS

```c
#include <pthread.h>

int pthread_join(pthread_t thread, void **value_ptr);
```

## DESCRIPTION

The `pthread_join()` function shall suspend execution of the calling thread until the target thread terminates, unless the target thread has already terminated. On return from a successful `pthread_join()` call with a non-NULL `value_ptr` argument, the value passed to `pthread_exit()` by the terminating thread shall be made available in the location referenced by `value_ptr`. When a `pthread_join()` returns successfully, the target thread has been terminated. The results of multiple simultaneous calls to `pthread_join()` specifying the same target thread are undefined. If the thread calling `pthread_join()` is canceled, then the target thread shall not be detached.

It is unspecified whether a zombie thread counts against {PTHREAD_THREADS_MAX}.

The behavior is undefined if the value specified by the thread argument to `pthread_join()` does not refer to a joinable thread.

The behavior is undefined if the value specified by the thread argument to `pthread_join()` refers to the calling thread.

If thread refers to a thread that was created using `thrd_create()` and the thread terminates, or has already terminated, by returning from its start routine, the behavior of `pthread_join()` is undefined. If thread refers to a thread that terminates, or has already terminated, by calling `thrd_exit()`, the behavior of `pthread_join()` is undefined.

## RETURN VALUE

If successful, the `pthread_join()` function shall return zero; otherwise, an error number shall be returned to indicate the error.

## ERRORS

The `pthread_join()` function may fail if:

- **[EDEADLK]** A deadlock was detected.

The `pthread_join()` function shall not return an error code of [EINTR].

## EXAMPLES

An example of thread creation and deletion follows:

```c
typedef struct {
    int *ar;
    long n;
} subarray;

void *
incer(void *arg)
{
    long i;

    for (i = 0; i < ((subarray *)arg)->n; i++)
        ((subarray *)arg)->ar[i]++;
}

int main(void)
{
    int        ar[1000000];
    pthread_t  th1, th2;
    subarray   sb1, sb2;

    sb1.ar = &ar[0];
    sb1.n  = 500000;
    (void) pthread_create(&th1, NULL, incer, &sb1);

    sb2.ar = &ar[500000];
    sb2.n  = 500000;
    (void) pthread_create(&th2, NULL, incer, &sb2);

    (void) pthread_join(th1, NULL);
    (void) pthread_join(th2, NULL);
    return 0;
}
```

## APPLICATION USAGE

None.

## RATIONALE

The `pthread_join()` function is a convenience that has proven useful in multi-threaded applications. It is true that a programmer could simulate this function if it were not provided by passing extra state as part of the argument to the `start_routine()`. The terminating thread would set a flag to indicate termination and broadcast a condition that is part of that state; a joining thread would wait on that condition variable. While such a technique would allow a thread to wait on more complex conditions (for example, waiting for multiple threads to terminate), waiting on individual thread termination is considered widely useful. Also, including the `pthread_join()` function in no way precludes a programmer from coding such complex waits. Thus, while not a primitive, including `pthread_join()` in this volume of POSIX.1-2024 was considered valuable.

The `pthread_join()` function provides a simple mechanism allowing an application to wait for a thread to terminate. After the thread terminates, the application may then choose to clean up resources that were used by the thread. For instance, after `pthread_join()` returns, any application-provided stack storage could be reclaimed.

The `pthread_join()` or `pthread_detach()` function should eventually be called for every thread that is created with the detachstate attribute set to PTHREAD_CREATE_JOINABLE so that storage associated with the thread may be reclaimed.

The interaction between `pthread_join()` and cancellation is well-defined for the following reasons:

- The `pthread_join()` function, like all other non-async-cancel-safe functions, can only be called with deferred cancelability type.
- Cancellation cannot occur in the disabled cancelability state.

Thus, only the default cancelability state need be considered. As specified, either the `pthread_join()` call is canceled, or it succeeds, but not both. The difference is obvious to the application, since either a cancellation handler is run or `pthread_join()` returns. There are no race conditions since `pthread_join()` was called in the deferred cancelability state.

If an implementation detects that the value specified by the thread argument to `pthread_join()` does not refer to a joinable thread, it is recommended that the function should fail and report an [EINVAL] error.

If an implementation detects that the value specified by the thread argument to `pthread_join()` refers to the calling thread, it is recommended that the function should fail and report an [EDEADLK] error.

If an implementation detects use of a thread ID after the end of its lifetime, it is recommended that the function should fail and report an [ESRCH] error.

The `pthread_join()` function cannot be used to obtain the exit status of a thread that was created using `thrd_create()` and which terminates by returning from its start routine, or of a thread that terminates by calling `thrd_exit()`, because such threads have an **int** exit status, instead of the **void \*** that `pthread_join()` returns via its value_ptr argument.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- `pthread_create()`
- `thrd_create()`
- `thrd_exit()`
- `wait()`

* XBD 4.15.2 Memory Synchronization, `<pthread.h>`

## CHANGE HISTORY

**First released in Issue 5.** Included for alignment with the POSIX Threads Extension.

**Issue 6**

The `pthread_join()` function is marked as part of the Threads option.

IEEE Std 1003.1-2001/Cor 2-2004, item XSH/TC2/D6/97 is applied, updating the ERRORS section so that the [EINVAL] error is made optional and the words "the implementation has detected" are removed from it.

**Issue 7**

The `pthread_join()` function is moved from the Threads option to the Base.

Austin Group Interpretation 1003.1-2001 #142 is applied, removing the [ESRCH] error condition.

The [EINVAL] error for a non-joinable thread is removed; this condition results in undefined behavior.

The [EDEADLK] error for the calling thread is removed; this condition results in undefined behavior.

**Issue 8**

Austin Group Defect 792 is applied, changing "a thread that has exited but remains unjoined" to "a zombie thread".

Austin Group Defect 1302 is applied, updating the page to account for the addition of `<threads.h>` interfaces.

---

*The Open Group Base Specifications Issue 8*
*IEEE Std 1003.1-2024*
*Copyright © 2001-2024 The IEEE and The Open Group*