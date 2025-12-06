# pthread_attr_getstacksize, pthread_attr_setstacksize — get and set the stacksize attribute

## SYNOPSIS

```c
#include <pthread.h>

int pthread_attr_getstacksize(const pthread_attr_t *restrict attr,
                              size_t *restrict stacksize);

int pthread_attr_setstacksize(pthread_attr_t *attr, size_t stacksize);
```

## DESCRIPTION

The `pthread_attr_getstacksize()` and `pthread_attr_setstacksize()` functions, respectively, shall get and set the thread creation *stacksize* attribute in the *attr* object.

The *stacksize* attribute shall define the minimum stack size (in bytes) allocated for the created threads stack.

The behavior is undefined if the value specified by the *attr* argument to `pthread_attr_getstacksize()` or `pthread_attr_setstacksize()` does not refer to an initialized thread attributes object.

## RETURN VALUE

Upon successful completion, `pthread_attr_getstacksize()` and `pthread_attr_setstacksize()` shall return a value of 0; otherwise, an error number shall be returned to indicate the error.

The `pthread_attr_getstacksize()` function stores the *stacksize* attribute value in *stacksize* if successful.

## ERRORS

The `pthread_attr_setstacksize()` function shall fail if:

- **[EINVAL]**
  The value of *stacksize* is less than {PTHREAD_STACK_MIN} or exceeds a system-imposed limit.

These functions shall not return an error code of [EINTR].

## EXAMPLES

None.

## APPLICATION USAGE

None.

## RATIONALE

If an implementation detects that the value specified by the *attr* argument to `pthread_attr_getstacksize()` or `pthread_attr_setstacksize()` does not refer to an initialized thread attributes object, it is recommended that the function should fail and report an [EINVAL] error.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- [`pthread_attr_destroy()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_attr_destroy.html)
- [`pthread_attr_getdetachstate()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_attr_getdetachstate.html)
- [`pthread_create()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_create.html)
- XBD [`<limits.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/limits.h.html)
- XBD [`<pthread.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/pthread.h.html)

## CHANGE HISTORY

### First released in Issue 5
Included for alignment with the POSIX Threads Extension.

### Issue 6
- The `pthread_attr_getstacksize()` and `pthread_attr_setstacksize()` functions are marked as part of the Threads and Thread Stack Size Attribute options.
- The **restrict** keyword is added to the `pthread_attr_getstacksize()` prototype for alignment with the ISO/IEC 9899:1999 standard.
- IEEE Std 1003.1-2001/Cor 1-2002, item XSH/TC1/D6/43 is applied, correcting the margin code in the SYNOPSIS from TSA to TSS and updating the CHANGE HISTORY from "Thread Stack Address Attribute" option to "Thread Stack Size Attribute" option.
- IEEE Std 1003.1-2001/Cor 2-2004, item XSH/TC2/D6/87 is applied, updating the ERRORS section to include optional errors for the case when *attr* refers to an uninitialized thread attribute object.

### Issue 7
- The `pthread_attr_getstacksize()` and `pthread_attr_setstacksize()` functions are marked only as part of the Thread Stack Size Attribute option as the Threads option is now part of the Base.
- The [EINVAL] error for an uninitialized thread attributes object is removed; this condition results in undefined behavior.
- POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0265 [757] is applied.

---
