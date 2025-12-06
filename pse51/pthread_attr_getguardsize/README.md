# pthread_attr_getguardsize, pthread_attr_setguardsize — get and set the thread guardsize attribute

## SYNOPSIS

```c
#include <pthread.h>

int pthread_attr_getguardsize(const pthread_attr_t *restrict attr,
                             size_t *restrict guardsize);

int pthread_attr_setguardsize(pthread_attr_t *attr,
                             size_t guardsize);
```

## DESCRIPTION

The `pthread_attr_getguardsize()` function shall get the `guardsize` attribute in the `attr` object. This attribute shall be returned in the `guardsize` parameter.

The `pthread_attr_setguardsize()` function shall set the `guardsize` attribute in the `attr` object. The new value of this attribute shall be obtained from the `guardsize` parameter. If `guardsize` is zero, a guard area shall not be provided for threads created with `attr`. If `guardsize` is greater than zero, a guard area of at least size `guardsize` bytes shall be provided for each thread created with `attr`.

The `guardsize` attribute controls the size of the guard area for the created thread's stack. The `guardsize` attribute provides protection against overflow of the stack pointer. If a thread's stack is created with guard protection, the implementation allocates extra memory at the overflow end of the stack as a buffer against stack overflow of the stack pointer. If an application overflows into this buffer an error shall result (possibly in a `SIGSEGV` signal being delivered to the thread).

A conforming implementation may round up the value contained in `guardsize` to a multiple of the configurable system variable `{PAGESIZE}` (see `<sys/mman.h>`). If an implementation rounds up the value of `guardsize` to a multiple of `{PAGESIZE}`, a call to `pthread_attr_getguardsize()` specifying `attr` shall store in the `guardsize` parameter the guard size specified by the previous `pthread_attr_setguardsize()` function call.

The default value of the `guardsize` attribute is implementation-defined.

If the `stackaddr` attribute has been set (that is, the caller is allocating and managing its own thread stacks), the `guardsize` attribute shall be ignored and no protection shall be provided by the implementation. It is the responsibility of the application to manage stack overflow along with stack allocation and management in this case.

The behavior is undefined if the value specified by the `attr` argument to `pthread_attr_getguardsize()` or `pthread_attr_setguardsize()` does not refer to an initialized thread attributes object.

## RETURN VALUE

If successful, the `pthread_attr_getguardsize()` and `pthread_attr_setguardsize()` functions shall return zero; otherwise, an error number shall be returned to indicate the error.

## ERRORS

These functions shall fail if:

- **EINVAL**
  - The parameter `guardsize` is invalid.

These functions shall not return an error code of `[EINTR]`.

## EXAMPLES

### Retrieving the guardsize Attribute

This example shows how to obtain the `guardsize` attribute of a thread attribute object.

```c
#include <pthread.h>

pthread_attr_t thread_attr;
size_t guardsize;
int rc;

/* code initializing thread_attr */
...

rc = pthread_attr_getguardsize(&thread_attr, &guardsize);
if (rc != 0) {
    /* handle error */
    ...
}
else {
    if (guardsize > 0) {
        /* a guard area of at least guardsize bytes is provided */
        ...
    }
    else {
        /* no guard area provided */
        ...
    }
}
```

## APPLICATION USAGE

None.

## RATIONALE

The `guardsize` attribute is provided to the application for two reasons:

1. Overflow protection can potentially result in wasted system resources. An application that creates a large number of threads, and which knows its threads never overflow their stack, can save system resources by turning off guard areas.

2. When threads allocate large data structures on the stack, large guard areas may be needed to detect stack overflow.

The default size of the guard area is left implementation-defined since on systems supporting very large page sizes, the overhead might be substantial if at least one guard page is required by default.

If an implementation detects that the value specified by the `attr` argument to `pthread_attr_getguardsize()` or `pthread_attr_setguardsize()` does not refer to an initialized thread attributes object, it is recommended that the function should fail and report an `[EINVAL]` error.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- `<pthread.h>`
- `<sys/mman.h>`

## CHANGE HISTORY

### First released in Issue 5.

### Issue 6

- In the ERRORS section, a third `[EINVAL]` error condition is removed as it is covered by the second error condition.
- The `restrict` keyword is added to the `pthread_attr_getguardsize()` prototype for alignment with the ISO/IEC 9899:1999 standard.
- IEEE Std 1003.1-2001/Cor 2-2004, item XSH/TC2/D6/74 is applied, updating the ERRORS section to remove the `[EINVAL]` error ("The attribute `attr` is invalid."), and replacing it with the optional `[EINVAL]` error.
- IEEE Std 1003.1-2001/Cor 2-2004, item XSH/TC2/D6/76 is applied, adding the example to the EXAMPLES section.

### Issue 7

- SD5-XSH-ERN-111 is applied, removing the reference to the `stack` attribute in the DESCRIPTION.
- SD5-XSH-ERN-175 is applied, updating the DESCRIPTION to note that the default size of the guard area is implementation-defined.
- The `pthread_attr_getguardsize()` and `pthread_attr_setguardsize()` functions are moved from the XSI option to the Base.
- The `[EINVAL]` error for an uninitialized thread attributes object is removed; this condition results in undefined behavior.

---
