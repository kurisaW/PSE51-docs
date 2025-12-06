# pthread_mutexattr_gettype, pthread_mutexattr_settype — get and set the mutex type attribute

## SYNOPSIS

```c
#include <pthread.h>

int pthread_mutexattr_gettype(const pthread_mutexattr_t *restrict attr,
                              int *restrict type);

int pthread_mutexattr_settype(pthread_mutexattr_t *attr, int type);
```

## DESCRIPTION

The `pthread_mutexattr_gettype()` and `pthread_mutexattr_settype()` functions, respectively, shall get and set the mutex **type** attribute. This attribute is set in the **type** parameter to these functions. The default value of the **type** attribute is `PTHREAD_MUTEX_DEFAULT`.

The type of mutex is contained in the **type** attribute of the mutex attributes. Valid mutex types include:

- `PTHREAD_MUTEX_NORMAL`
- `PTHREAD_MUTEX_ERRORCHECK`
- `PTHREAD_MUTEX_RECURSIVE`
- `PTHREAD_MUTEX_DEFAULT`

The mutex type affects the behavior of calls which lock and unlock the mutex. See `pthread_mutex_lock()` for details. An implementation may map `PTHREAD_MUTEX_DEFAULT` to one of the other mutex types.

The behavior is undefined if the value specified by the **attr** argument to `pthread_mutexattr_gettype()` or `pthread_mutexattr_settype()` does not refer to an initialized mutex attributes object.

## RETURN VALUE

Upon successful completion, the `pthread_mutexattr_gettype()` function shall return zero and store the value of the **type** attribute of **attr** into the object referenced by the **type** parameter. Otherwise, an error shall be returned to indicate the error.

If successful, the `pthread_mutexattr_settype()` function shall return zero; otherwise, an error number shall be returned to indicate the error.

## ERRORS

The `pthread_mutexattr_settype()` function shall fail if:

**[EINVAL]**
The value **type** is invalid.

These functions shall not return an error code of `[EINTR]`.

---

*The following sections are informative.*

## EXAMPLES

None.

## APPLICATION USAGE

It is advised that an application should not use a `PTHREAD_MUTEX_RECURSIVE` mutex with condition variables because the implicit unlock performed in a `pthread_cond_clockwait()`, `pthread_cond_timedwait()`, or `pthread_cond_wait()` call may not actually release the mutex (if it had been locked multiple times). If this happens, no other thread can satisfy the condition of the predicate.

## RATIONALE

If an implementation detects that the value specified by the **attr** argument to `pthread_mutexattr_gettype()` or `pthread_mutexattr_settype()` does not refer to an initialized mutex attributes object, it is recommended that the function should fail and report an `[EINVAL]` error.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- `pthread_cond_clockwait()`
- `pthread_mutex_lock()`
- `<pthread.h>`

## CHANGE HISTORY

### First released in Issue 5.

### Issue 6

The Open Group Corrigendum U033/3 is applied. The SYNOPSIS for `pthread_mutexattr_gettype()` is updated so that the first argument is of type **const pthread_mutexattr_t \***.

The **restrict** keyword is added to the `pthread_mutexattr_gettype()` prototype for alignment with the ISO/IEC 9899:1999 standard.

### Issue 7

The `pthread_mutexattr_gettype()` and `pthread_mutexattr_settype()` functions are moved from the XSI option to the Base.

The `[EINVAL]` error for an uninitialized mutex attributes object is removed; this condition results in undefined behavior.

POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0464 [121] is applied.

### Issue 8

Austin Group Defect 1216 is applied, adding `pthread_cond_clockwait()`.

---
