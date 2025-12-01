# flockfile, ftrylockfile, funlockfile — stdio locking functions

## SYNOPSIS

```c
#include <stdio.h>

void flockfile(FILE *file);
int ftrylockfile(FILE *file);
void funlockfile(FILE *file);
```

## DESCRIPTION

These functions shall provide for explicit application-level locking of the locks associated with standard I/O streams (see 2.5 Standard I/O Streams). These functions can be used by a thread to delineate a sequence of I/O statements that are executed as a unit.

The `flockfile()` function shall acquire for a thread ownership of a (`FILE *`) object.

The `ftrylockfile()` function shall acquire for a thread ownership of a (`FILE *`) object if the object is available; `ftrylockfile()` is a non-blocking version of `flockfile()`.

The `funlockfile()` function shall relinquish the ownership granted to the thread. The behavior is undefined if a thread other than the current owner calls the `funlockfile()` function.

The functions shall behave as if there is a lock count associated with each (`FILE *`) object. This count is implicitly initialized to zero when the (`FILE *`) object is created. The (`FILE *`) object is unlocked when the count is zero. When the count is positive, a single thread owns the (`FILE *`) object. When the `flockfile()` function is called, if the count is zero or if the count is positive and the caller owns the (`FILE *`) object, the count shall be incremented. Otherwise, the calling thread shall be suspended, waiting for the count to return to zero. Each call to `funlockfile()` shall decrement the count. This allows matching calls to `flockfile()` (or successful calls to `ftrylockfile()`) and `funlockfile()` to be nested.

## RETURN VALUE

None for `flockfile()` and `funlockfile()`.

The `ftrylockfile()` function shall return zero for success and non-zero to indicate that the lock cannot be acquired.

## ERRORS

No errors are defined.

## APPLICATION USAGE

Applications using these functions may be subject to priority inversion, as discussed in XBD 3.275 Priority Inversion.

A call to `exit()` can block until locked streams are unlocked because a thread having ownership of a (`FILE*`) object blocks all function calls that reference that (`FILE*`) object (except those with names ending in _unlocked) from other threads, including calls to `exit()`.

Note: a FILE lock is not a file lock (see XBD 3.143 File Lock).

## RATIONALE

The `flockfile()` and `funlockfile()` functions provide an orthogonal mutual-exclusion lock for each FILE. The `ftrylockfile()` function provides a non-blocking attempt to acquire a FILE lock, analogous to `pthread_mutex_trylock()`.

These locks behave as if they are the same as those used internally by stdio for thread-safety. This both provides thread-safety of these functions without requiring a second level of internal locking and allows functions in stdio to be implemented in terms of other stdio functions.

Application developers and implementors should be aware that there are potential deadlock problems on FILE objects. For example, the line-buffered flushing semantics of stdio (requested via {_IOLBF}) require that certain input operations sometimes cause the buffered contents of implementation-defined line-buffered output streams to be flushed. If two threads each hold the lock on the other's FILE, deadlock ensues. This type of deadlock can be avoided by acquiring FILE locks in a consistent order. In particular, the line-buffered output stream deadlock can typically be avoided by acquiring locks on input streams before locks on output streams if a thread would be acquiring both.

In summary, threads sharing stdio streams with other threads can use `flockfile()` and `funlockfile()` to cause sequences of I/O performed by a single thread to be kept bundled. The only case where the use of `flockfile()` and `funlockfile()` is required is to provide a scope protecting uses of the *_unlocked functions/macros. This moves the cost/performance tradeoff to the optimal point.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- `exit()`
- `getc_unlocked()`
- XBD 3.275 Priority Inversion
- `<stdio.h>`

## CHANGE HISTORY

### First released in Issue 5
Included for alignment with the POSIX Threads Extension.

### Issue 6
These functions are marked as part of the Thread-Safe Functions option.

### Issue 7
The `flockfile()`, `ftrylockfile()`, and `funlockfile()` functions are moved from the Thread-Safe Functions option to the Base.

POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0140 [118] is applied.

POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0116 [611] is applied.

### Issue 8
Austin Group Defect 1118 is applied, clarifying that a FILE lock is not a file lock.

Austin Group Defect 1302 is applied, replacing parts of the text with a reference to 2.5 Standard I/O Streams.