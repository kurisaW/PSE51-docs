# pthread_attr_getstackaddr, pthread_attr_setstackaddr

## SYNOPSIS

```c
[OB] #include <pthread.h>

int pthread_attr_getstackaddr(const pthread_attr_t *restrict attr,
                             void **restrict stackaddr);

int pthread_attr_setstackaddr(pthread_attr_t *attr, void *stackaddr);
```

## DESCRIPTION

The `pthread_attr_getstackaddr()` and `pthread_attr_setstackaddr()` functions, respectively, shall get and set the thread creation `stackaddr` attribute in the `attr` object.

The `stackaddr` attribute specifies the location of storage to be used for the created thread's stack. The size of the storage shall be at least {PTHREAD_STACK_MIN}.

## RETURN VALUE

Upon successful completion, `pthread_attr_getstackaddr()` and `pthread_attr_setstackaddr()` shall return a value of 0; otherwise, an error number shall be returned to indicate the error.

The `pthread_attr_getstackaddr()` function stores the `stackaddr` attribute value in `stackaddr` if successful.

## ERRORS

These functions may fail if:

- **[EINVAL]**
  The value specified by `attr` does not refer to an initialized thread attribute object.

These functions shall not return an error code of [EINTR].

## APPLICATION USAGE

The specification of the `stackaddr` attribute presents several ambiguities that make portable use of these interfaces impossible. The description of the single address parameter as a "stack" does not specify a particular relationship between the address and the "stack" implied by that address. For example, the address may be taken as the low memory address of a buffer intended for use as a stack, or it may be taken as the address to be used as the initial stack pointer register value for the new thread. These two are not the same except for a machine on which the stack grows "up" from low memory to high, and on which a "push" operation first stores the value in memory and then increments the stack pointer register. Further, on a machine where the stack grows "down" from high memory to low, interpretation of the address as the "low memory" address requires a determination of the intended size of the stack. IEEE Std 1003.1-2001 has introduced the new interfaces [`pthread_attr_setstack()`](pthread_attr_setstack.html) and [`pthread_attr_getstack()`](pthread_attr_getstack.html) to resolve these ambiguities.

After a successful call to `pthread_attr_setstackaddr()`, the storage area specified by the `stackaddr` parameter is under the control of the implementation, as described in *Use of Application-Managed Thread Stacks*.

## SEE ALSO

- [`pthread_attr_destroy()`](pthread_attr_destroy.html)
- [`pthread_attr_getdetachstate()`](pthread_attr_getdetachstate.html)
- [`pthread_attr_getstack()`](pthread_attr_getstack.html)
- [`pthread_attr_getstacksize()`](pthread_attr_getstacksize.html)
- [`pthread_attr_setstack()`](pthread_attr_setstack.html)
- [`pthread_create()`](pthread_create.html)
- `<limits.h>`
- `<pthread.h>`

## CHANGE HISTORY

First released in Issue 5. Included for alignment with the POSIX Threads Extension.

### Issue 6

The `pthread_attr_getstackaddr()` and `pthread_attr_setstackaddr()` functions are marked as part of the Threads and Thread Stack Address Attribute options.

The `restrict` keyword is added to the `pthread_attr_getstackaddr()` prototype for alignment with the ISO/IEC 9899:1999 standard.

These functions are marked obsolescent.

IEEE Std 1003.1-2001/Cor 2-2004, item XSH/TC2/D6/85 is applied, updating the APPLICATION USAGE section to refer to *Use of Application-Managed Thread Stacks*.

IEEE Std 1003.1-2001/Cor 2-2004, item XSH/TC2/D6/86 is applied, updating the ERRORS section to include optional errors for the case when `attr` refers to an uninitialized thread attribute object.

---
