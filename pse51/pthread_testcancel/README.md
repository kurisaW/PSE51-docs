# pthread_setcancelstate, pthread_setcanceltype, pthread_testcancel

## SYNOPSIS

```c
#include <pthread.h>

int pthread_setcancelstate(int state, int *oldstate);
int pthread_setcanceltype(int type, int *oldtype);
void pthread_testcancel(void);
```

## DESCRIPTION

The `pthread_setcancelstate()` function shall atomically both set the calling thread's cancelability state to the indicated `state` and return the previous cancelability state at the location referenced by `oldstate`. Legal values for `state` are `PTHREAD_CANCEL_ENABLE` and `PTHREAD_CANCEL_DISABLE`.

The `pthread_setcanceltype()` function shall atomically both set the calling thread's cancelability type to the indicated `type` and return the previous cancelability type at the location referenced by `oldtype`. Legal values for `type` are `PTHREAD_CANCEL_DEFERRED` and `PTHREAD_CANCEL_ASYNCHRONOUS`.

The cancelability state and type of any newly created threads, including the thread in which `main()` was first invoked, shall be `PTHREAD_CANCEL_ENABLE` and `PTHREAD_CANCEL_DEFERRED` respectively.

The `pthread_testcancel()` function shall create a cancellation point in the calling thread. The `pthread_testcancel()` function shall have no effect if cancelability is disabled.

The `pthread_setcancelstate()` function shall be async-signal-safe.

## RETURN VALUE

If successful, the `pthread_setcancelstate()` and `pthread_setcanceltype()` functions shall return zero; otherwise, an error number shall be returned to indicate the error.

## ERRORS

The `pthread_setcancelstate()` function may fail if:

- **[EINVAL]**
  - The specified state is not `PTHREAD_CANCEL_ENABLE` or `PTHREAD_CANCEL_DISABLE`.

The `pthread_setcanceltype()` function may fail if:

- **[EINVAL]**
  - The specified type is not `PTHREAD_CANCEL_DEFERRED` or `PTHREAD_CANCEL_ASYNCHRONOUS`.

These functions shall not return an error code of `[EINTR]`.

---

*The following sections are informative.*

## EXAMPLES

None.

## APPLICATION USAGE

In order to write a signal handler for an asynchronous signal which can run safely in a cancellable thread, `pthread_setcancelstate()` must be used to disable cancellation for the duration of any calls that the signal handler makes which are cancellation points. However, earlier versions of the standard did not permit strictly conforming applications to call `pthread_setcancelstate()` from a signal handler since it was not required to be async-signal-safe. On non-conforming implementations where `pthread_setcancelstate()` is not async-signal-safe, alternatives are to ensure either that the corresponding signals are blocked during execution of functions that are not async-cancel-safe or that cancellation is disabled during times when those signals could be delivered.

## RATIONALE

The `pthread_setcancelstate()` and `pthread_setcanceltype()` functions control the points at which a thread may be asynchronously canceled. For cancellation control to be usable in modular fashion, some rules need to be followed.

An object can be considered to be a generalization of a procedure. It is a set of procedures and global variables written as a unit and called by clients not known by the object. Objects may depend on other objects.

First, cancelability should only be disabled on entry to an object, never explicitly enabled. On exit from an object, the cancelability state should always be restored to its value on entry to the object.

This follows from a modularity argument: if the client of an object (or the client of an object that uses that object) has disabled cancelability, it is because the client does not want to be concerned about cleaning up if the thread is canceled while executing some sequence of actions. If an object is called in such a state and it enables cancelability and a cancellation request is pending for that thread, then the thread is canceled, contrary to the wish of the client that disabled.

Second, the cancelability type may be explicitly set to either deferred or asynchronous upon entry to an object. But as with the cancelability state, on exit from an object the cancelability type should always be restored to its value on entry to the object.

Finally, only functions that are cancel-safe may be called from a thread that is asynchronously cancelable.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- [`pthread_cancel()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_cancel.html)
- XBD [`<pthread.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/pthread.h.html)

## CHANGE HISTORY

First released in Issue 5. Included for alignment with the POSIX Threads Extension.

### Issue 6

The `pthread_setcancelstate()`, `pthread_setcanceltype()`, and `pthread_testcancel()` functions are marked as part of the Threads option.

### Issue 7

The `pthread_setcancelstate()`, `pthread_setcanceltype()`, and `pthread_testcancel()` functions are moved from the Threads option to the Base.

POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0294 [622] and XSH/TC2-2008/0295 [615] are applied.

### Issue 8

Austin Group Defect 841 is applied, requiring `pthread_setcancelstate()` to be async-signal-safe.

