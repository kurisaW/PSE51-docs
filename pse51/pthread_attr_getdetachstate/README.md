# pthread_attr_getdetachstate, pthread_attr_setdetachstate — get and set the detachstate attribute

## SYNOPSIS

```c
#include <pthread.h>

int pthread_attr_getdetachstate(const pthread_attr_t *attr, int *detachstate);
int pthread_attr_setdetachstate(pthread_attr_t *attr, int detachstate);
```

## DESCRIPTION

The `detachstate` attribute controls whether the thread is created in a detached state. If the thread is created detached, then use of the ID of the newly created thread by the `pthread_detach()` or `pthread_join()` function is an error.

The `pthread_attr_getdetachstate()` and `pthread_attr_setdetachstate()` functions, respectively, shall get and set the `detachstate` attribute in the `attr` object.

For `pthread_attr_getdetachstate()`, `detachstate` shall be set to either `PTHREAD_CREATE_DETACHED` or `PTHREAD_CREATE_JOINABLE`.

For `pthread_attr_setdetachstate()`, the application shall set `detachstate` to either `PTHREAD_CREATE_DETACHED` or `PTHREAD_CREATE_JOINABLE`.

A value of `PTHREAD_CREATE_DETACHED` shall cause all threads created with `attr` to be in the detached state, whereas using a value of `PTHREAD_CREATE_JOINABLE` shall cause all threads created with `attr` to be in the joinable state. The default value of the `detachstate` attribute shall be `PTHREAD_CREATE_JOINABLE`.

The behavior is undefined if the value specified by the `attr` argument to `pthread_attr_getdetachstate()` or `pthread_attr_setdetachstate()` does not refer to an initialized thread attributes object.

## RETURN VALUE

Upon successful completion, `pthread_attr_getdetachstate()` and `pthread_attr_setdetachstate()` shall return a value of 0; otherwise, an error number shall be returned to indicate the error.

The `pthread_attr_getdetachstate()` function stores the value of the `detachstate` attribute in `detachstate` if successful.

## ERRORS

The `pthread_attr_setdetachstate()` function shall fail if:

- **[EINVAL]**
  - The value of `detachstate` was not valid

These functions shall not return an error code of `[EINTR]`.

---

*The following sections are informative.*

## EXAMPLES

### Retrieving the detachstate Attribute

This example shows how to obtain the `detachstate` attribute of a thread attribute object.

```c
#include <pthread.h>

pthread_attr_t thread_attr;
int            detachstate;
int            rc;

/* code initializing thread_attr */
...

rc = pthread_attr_getdetachstate(&thread_attr, &detachstate);
if (rc != 0) {
    /* handle error */
    ...
}
else {
    /* legal values for detachstate are:
     * PTHREAD_CREATE_DETACHED or PTHREAD_CREATE_JOINABLE
     */
     ...
}
```

## APPLICATION USAGE

None.

## RATIONALE

If an implementation detects that the value specified by the `attr` argument to `pthread_attr_getdetachstate()` or `pthread_attr_setdetachstate()` does not refer to an initialized thread attributes object, it is recommended that the function should fail and report an `[EINVAL]` error.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- `pthread_attr_destroy()`
- `pthread_attr_getstacksize()`
- `pthread_create()`
- `<pthread.h>`

## CHANGE HISTORY

First released in Issue 5. Included for alignment with the POSIX Threads Extension.

### Issue 6

The `pthread_attr_setdetachstate()` and `pthread_attr_getdetachstate()` functions are marked as part of the Threads option.

The normative text is updated to avoid use of the term "must" for application requirements.

IEEE Std 1003.1-2001/Cor 2-2004, item XSH/TC2/D6/72 is applied, adding the example to the EXAMPLES section.

IEEE Std 1003.1-2001/Cor 2-2004, item XSH/TC2/D6/73 is applied, updating the ERRORS section to include the optional `[EINVAL]` error.

### Issue 7

The `pthread_attr_setdetachstate()` and `pthread_attr_getdetachstate()` functions are moved from the Threads option to the Base.

The `[EINVAL]` error for an uninitialized thread attributes object is removed; this condition results in undefined behavior.

