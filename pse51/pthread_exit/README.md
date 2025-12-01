# pthread_exit — thread termination

## SYNOPSIS

```c
#include <pthread.h>

_Noreturn void pthread_exit(void *value_ptr);
```

## DESCRIPTION

The `pthread_exit()` function shall terminate the calling thread and make the value `value_ptr` available to any successful join with the terminating thread. Any cancellation cleanup handlers that have been pushed and not yet popped shall be popped in the reverse order that they were pushed and then executed. After all cancellation cleanup handlers have been executed, if the thread has any thread-specific data (whether associated with key type `tss_t` or `pthread_key_t`), appropriate destructor functions shall be called in an unspecified order. Thread termination does not release any application visible process resources, including, but not limited to, mutexes and file descriptors, nor does it perform any process-level cleanup actions, including, but not limited to, calling any `atexit()` routines that may exist.

An implicit call to `pthread_exit()` is made when a thread that was not created using `thrd_create()`, and is not the thread in which `main()` was first invoked, returns from the start routine that was used to create it. The function's return value shall serve as the thread's exit status.

The behavior of `pthread_exit()` is undefined if called from a cancellation cleanup handler or destructor function that was invoked as a result of either an implicit or explicit call to `pthread_exit()`.

After a thread has terminated, the result of access to local (auto) variables of the thread is undefined. Thus, references to local variables of the exiting thread should not be used for the `pthread_exit()` `value_ptr` parameter value.

The process shall exit with an exit status of 0 after the last thread has been terminated. The behavior shall be as if the implementation called `_exit()` with a zero argument at thread termination time.

## RETURN VALUE

The `pthread_exit()` function cannot return to its caller.

## ERRORS

No errors are defined.

## EXAMPLES

None.

## APPLICATION USAGE

Calls to `pthread_exit()` should not be made from threads created using `thrd_create()`, as their exit status has a different type (`int` instead of `void *`). If `pthread_exit()` is called from the initial thread and it is not the last thread to terminate, other threads should not try to obtain its exit status using `thrd_join()`.

## RATIONALE

The normal mechanism by which a thread that was started using `pthread_create()` terminates is to return from the routine that was specified in the `pthread_create()` call that started it. The `pthread_exit()` function provides the capability for a thread to terminate without requiring a return from the start routine of that thread, thereby providing a function analogous to `_exit()`.

Regardless of the method of thread termination, any cancellation cleanup handlers that have been pushed and not yet popped are executed, and the destructors for any existing thread-specific data are executed. This volume of POSIX.1-2024 requires that cancellation cleanup handlers be popped and called in order. After all cancellation cleanup handlers have been executed, thread-specific data destructors are called, in an unspecified order, for each item of thread-specific data that exists in the thread. This ordering is necessary because cancellation cleanup handlers may rely on thread-specific data.

As the meaning of the status is determined by the application (except when the thread has been canceled, in which case it is PTHREAD_CANCELED), the implementation has no idea what an illegal status value is, which is why no address error checking is done.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- `_exit()`
- `pthread_create()`
- `pthread_join()`
- `pthread_key_create()`
- `thrd_create()`
- `thrd_exit()`
- `tss_create()`
- `<pthread.h>`

## CHANGE HISTORY

First released in Issue 5. Included for alignment with the POSIX Threads Extension.

### Issue 6

The `pthread_exit()` function is marked as part of the Threads option.

### Issue 7

The `pthread_exit()` function is moved from the Threads option to the Base.

### Issue 8

Austin Group Defect 1302 is applied, adding `_Noreturn` to the SYNOPSIS, and updating the page to account for the addition of `<threads.h>` interfaces.