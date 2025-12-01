# pthread_self — get the calling thread ID

## SYNOPSIS

```c
#include <pthread.h>

pthread_t pthread_self(void);
```

## DESCRIPTION

The `pthread_self()` function shall return the thread ID of the calling thread.

## RETURN VALUE

The `pthread_self()` function shall always be successful and no return value is reserved to indicate an error.

## ERRORS

No errors are defined.

*The following sections are informative.*

## EXAMPLES

None.

## APPLICATION USAGE

None.

## RATIONALE

The `pthread_self()` function provides a capability similar to the `getpid()` function for processes and the rationale is the same: the creation call does not provide the thread ID to the created thread.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- `pthread_create()`
- `pthread_equal()`
- `<pthread.h>`

## CHANGE HISTORY

First released in Issue 5. Included for alignment with the POSIX Threads Extension.

### Issue 6

The `pthread_self()` function is marked as part of the Threads option.

### Issue 7

Austin Group Interpretation 1003.1-2001 #063 is applied, updating the RETURN VALUE section.

The `pthread_self()` function is moved from the Threads option to the Base.

*End of informative text.*

---

*The Open Group Base Specifications Issue 8*
*IEEE Std 1003.1-2024*
*Copyright © 2001-2024 The IEEE and The Open Group*