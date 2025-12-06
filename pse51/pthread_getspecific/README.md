# pthread_getspecific, pthread_setspecific — thread-specific data management

## SYNOPSIS

```c
#include <pthread.h>

void *pthread_getspecific(pthread_key_t key);
int pthread_setspecific(pthread_key_t key, const void *value);
```

## DESCRIPTION

The `pthread_getspecific()` function shall return the value currently bound to the specified `key` on behalf of the calling thread.

The `pthread_setspecific()` function shall associate a thread-specific `value` with a `key` obtained via a previous call to `pthread_key_create()`. Different threads may bind different values to the same key. These values are typically pointers to blocks of dynamically allocated memory that have been reserved for use by the calling thread.

The effect of calling `pthread_getspecific()` or `pthread_setspecific()` with a `key` value not obtained from `pthread_key_create()` or after `key` has been deleted with `pthread_key_delete()` is undefined.

Both `pthread_getspecific()` and `pthread_setspecific()` may be called from a thread-specific data destructor function. A call to `pthread_getspecific()` for the thread-specific data key being destroyed shall return the value NULL, unless the value is changed (after the destructor starts) by a call to `pthread_setspecific()`. Calling `pthread_setspecific()` from a thread-specific data destructor routine may result either in lost storage (after at least PTHREAD_DESTRUCTOR_ITERATIONS attempts at destruction) or in an infinite loop.

Both functions may be implemented as macros.

## RETURN VALUE

The `pthread_getspecific()` function shall return the thread-specific data value associated with the given `key`. If no thread-specific data value is associated with `key`, then the value NULL shall be returned.

If successful, the `pthread_setspecific()` function shall return zero; otherwise, an error number shall be returned to indicate the error.

## ERRORS

No errors are returned from `pthread_getspecific()`.

The `pthread_setspecific()` function shall fail if:

- **[ENOMEM]**
  - Insufficient memory exists to associate the non-NULL value with the key.

The `pthread_setspecific()` function shall not return an error code of [EINTR].

## EXAMPLES

None.

## APPLICATION USAGE

None.

## RATIONALE

Performance and ease-of-use of `pthread_getspecific()` are critical for functions that rely on maintaining state in thread-specific data. Since no errors are required to be detected by it, and since the only error that could be detected is the use of an invalid key, the function to `pthread_getspecific()` has been designed to favor speed and simplicity over error reporting.

If an implementation detects that the value specified by the `key` argument to `pthread_setspecific()` does not refer to a a key value obtained from `pthread_key_create()` or refers to a key that has been deleted with `pthread_key_delete()`, it is recommended that the function should fail and report an [EINVAL] error.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- `pthread_key_create()`
- `<pthread.h>`

## CHANGE HISTORY

### First released in Issue 5
Included for alignment with the POSIX Threads Extension.

### Issue 6
- The `pthread_getspecific()` and `pthread_setspecific()` functions are marked as part of the Threads option.
- IEEE PASC Interpretation 1003.1c #3 (Part 6) is applied, updating the DESCRIPTION.
- IEEE Std 1003.1-2001/Cor 2-2004, item XSH/TC2/D6/96 is applied, updating the ERRORS section so that the [ENOMEM] error case is changed from "to associate the value with the key" to "to associate the non-NULL value with the key".

### Issue 7
- Austin Group Interpretation 1003.1-2001 #063 is applied, updating the ERRORS section.
- The `pthread_getspecific()` and `pthread_setspecific()` functions are moved from the Threads option to the Base.
- The [EINVAL] error for a key value not obtained from `pthread_key_create()` or a key deleted with `pthread_key_delete()` is removed; this condition results in undefined behavior.

---
