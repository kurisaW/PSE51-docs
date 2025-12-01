# pthread_detach — detach a thread

## SYNOPSIS

```c
#include <pthread.h>

int pthread_detach(pthread_t thread);
```

## DESCRIPTION

The `pthread_detach()` function shall change the thread `thread` from joinable to detached, indicating to the implementation that storage for the thread can be reclaimed when the thread terminates. If `thread` has not terminated, `pthread_detach()` shall not cause it to terminate, but shall prevent the thread from becoming a zombie thread when it does terminate.

The behavior is undefined if the value specified by the `thread` argument to `pthread_detach()` does not refer to a joinable thread.

## RETURN VALUE

If the call succeeds, `pthread_detach()` shall return 0; otherwise, an error number shall be returned to indicate the error.

## ERRORS

The `pthread_detach()` function shall not return an error code of `[EINTR]`.

*The following sections are informative.*

## EXAMPLES

None.

## APPLICATION USAGE

None.

## RATIONALE

The `pthread_join()` or `pthread_detach()` functions should eventually be called for every thread that is created so that storage associated with the thread may be reclaimed.

It has been suggested that a "detach" function is not necessary; the `detachstate` thread creation attribute is sufficient, since a thread need never be dynamically detached. However, need arises in at least two cases:

1. In a cancellation handler for a `pthread_join()` it is nearly essential to have a `pthread_detach()` function in order to detach the thread on which `pthread_join()` was waiting. Without it, it would be necessary to have the handler do another `pthread_join()` to attempt to detach the thread, which would both delay the cancellation processing for an unbounded period and introduce a new call to `pthread_join()`, which might itself need a cancellation handler. A dynamic detach is nearly essential in this case.

2. In order to detach the "initial thread" (as may be desirable in processes that set up server threads).

If an implementation detects that the value specified by the `thread` argument to `pthread_detach()` does not refer to a joinable thread, it is recommended that the function should fail and report an `[EINVAL]` error.

If an implementation detects use of a thread ID after the end of its lifetime, it is recommended that the function should fail and report an `[ESRCH]` error.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- `pthread_join()`
- XBD `<pthread.h>`

## CHANGE HISTORY

First released in Issue 5. Included for alignment with the POSIX Threads Extension.

### Issue 6

The `pthread_detach()` function is marked as part of the Threads option.

IEEE Std 1003.1-2001/Cor 2-2004, item XSH/TC2/D6/95 is applied, updating the ERRORS section so that the `[EINVAL]` and `[ESRCH]` error cases become optional.

### Issue 7

The `pthread_detach()` function is moved from the Threads option to the Base.

Austin Group Interpretation 1003.1-2001 #142 is applied, removing the `[ESRCH]` error condition.

The `[EINVAL]` error for a non-joinable thread is removed; this condition results in undefined behavior.

### Issue 8

Austin Group Defect 792 is applied, clarifying that detaching a live thread prevents it becoming a zombie thread when it terminates.

Austin Group Defect 1167 is applied, clarifying that a thread is no longer joinable after `pthread_detach()` has been called for it.

---

*The Open Group Base Specifications Issue 8*
*IEEE Std 1003.1-2024*
*Copyright © 2001-2024 The IEEE and The Open Group*

UNIX® is a registered Trademark of The Open Group.
POSIX™ is a Trademark of The IEEE.
Copyright © 2001-2024 The IEEE and The Open Group, All Rights Reserved