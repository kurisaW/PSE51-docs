# pthread_cancel

## NAME

pthread_cancel — cancel execution of a thread

## SYNOPSIS

```c
#include <pthread.h>

int pthread_cancel(pthread_t thread);
```

## DESCRIPTION

The `pthread_cancel()` function shall request that `thread` be canceled. The target thread's cancelability state and type determines when the cancellation takes effect. When the cancellation is acted on, the cancellation cleanup handlers for `thread` shall be called. When the last cancellation cleanup handler returns, the thread-specific data destructor functions shall be called for `thread`. When the last destructor function returns, `thread` shall be terminated. It shall not be an error to request cancellation of a zombie thread.

The cancellation processing in the target thread shall run asynchronously with respect to the calling thread returning from `pthread_cancel()`.

If `thread` refers to a thread that was created using `thrd_create()`, the behavior is undefined.

## RETURN VALUE

If successful, the `pthread_cancel()` function shall return zero; otherwise, an error number shall be returned to indicate the error.

## ERRORS

The `pthread_cancel()` function shall not return an error code of [EINTR].

---

*The following sections are informative.*

## EXAMPLES

None.

## APPLICATION USAGE

None.

## RATIONALE

Two alternative functions were considered for sending the cancellation notification to a thread. One would be to define a new SIGCANCEL signal that had the cancellation semantics when delivered; the other was to define the new `pthread_cancel()` function, which would trigger the cancellation semantics.

The advantage of a new signal was that so much of the delivery criteria were identical to that used when trying to deliver a signal that making cancellation notification a signal was seen as consistent. Indeed, many implementations implement cancellation using a special signal. On the other hand, there would be no signal functions that could be used with this signal except `pthread_kill()`, and the behavior of the delivered cancellation signal would be unlike any previously existing defined signal.

The benefits of a special function include the recognition that this signal would be defined because of the similar delivery criteria and that this is the only common behavior between a cancellation request and a signal. In addition, the cancellation delivery mechanism does not have to be implemented as a signal. There are also strong, if not stronger, parallels with language exception mechanisms than with signals that are potentially obscured if the delivery mechanism is visibly closer to signals.

In the end, it was considered that as there were so many exceptions to the use of the new signal with existing signals functions it would be misleading. A special function has resolved this problem. This function was carefully defined so that an implementation wishing to provide the cancellation functions on top of signals could do so. The special function also means that implementations are not obliged to implement cancellation with signals.

If an implementation detects use of a thread ID after the end of its lifetime, it is recommended that the function should fail and report an [ESRCH] error.

Historical implementations varied on the result of a `pthread_cancel()` with a thread ID indicating a zombie thread. Some indicated success with nothing further to do because the thread had already terminated, while others gave an error of [ESRCH]. Since the definition of thread lifetime in this standard covers zombie threads, the [ESRCH] error as described is inappropriate in this case and implementations that give this error do not conform.

Use of `pthread_cancel()` to cancel a thread that was created using `thrd_create()` is undefined because `thrd_join()` has no way to indicate a thread was cancelled. The standard developers considered adding a `thrd_canceled` enumeration constant that `thrd_join()` would return in this case. However, this return would be unexpected in code that is written to conform to the ISO C standard, and it would also not solve the problem that threads which use only ISO C `<threads.h>` interfaces (such as ones created by third party libraries written to conform to the ISO C standard) have no way to handle being cancelled, as the ISO C standard does not provide cancellation cleanup handlers.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- `pthread_exit()`
- `pthread_cond_clockwait()`
- `pthread_join()`
- `pthread_setcancelstate()`

XBD `<pthread.h>`

## CHANGE HISTORY

### First released in Issue 5. Included for alignment with the POSIX Threads Extension.

### Issue 6

The `pthread_cancel()` function is marked as part of the Threads option.

### Issue 7

The `pthread_cancel()` function is moved from the Threads option to the Base.

Austin Group Interpretation 1003.1-2001 #142 is applied, removing the [ESRCH] error condition.

### Issue 8

Austin Group Defect 792 is applied, adding a requirement that passing the thread ID of a zombie thread to `pthread_cancel()` is not treated as an error.

Austin Group Defect 1302 is applied, updating the page to account for the addition of `<threads.h>` interfaces.

---

