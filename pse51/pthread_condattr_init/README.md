# pthread_condattr_destroy, pthread_condattr_init

The Open Group Base Specifications Issue 8
IEEE Std 1003.1-2024
Copyright © 2001-2024 The IEEE and The Open Group

## NAME

pthread_condattr_destroy, pthread_condattr_init — destroy and initialize the condition variable attributes object

## SYNOPSIS

```c
#include <pthread.h>

int pthread_condattr_destroy(pthread_condattr_t *attr);
int pthread_condattr_init(pthread_condattr_t *attr);
```

## DESCRIPTION

The `pthread_condattr_destroy()` function shall destroy a condition variable attributes object; the object becomes, in effect, uninitialized. An implementation may cause `pthread_condattr_destroy()` to set the object referenced by `attr` to an invalid value. A destroyed `attr` attributes object can be reinitialized using `pthread_condattr_init()`; the results of otherwise referencing the object after it has been destroyed are undefined.

The `pthread_condattr_init()` function shall initialize a condition variable attributes object `attr` with the default value for all of the attributes defined by the implementation.

Results are undefined if `pthread_condattr_init()` is called specifying an already initialized `attr` attributes object.

After a condition variable attributes object has been used to initialize one or more condition variables, any function affecting the attributes object (including destruction) shall not affect any previously initialized condition variables.

This volume of POSIX.1-2024 requires two attributes, the **clock** attribute and the **process-shared** attribute.

Additional attributes, their default values, and the names of the associated functions to get and set those attribute values are implementation-defined.

The behavior is undefined if the value specified by the `attr` argument to `pthread_condattr_destroy()` does not refer to an initialized condition variable attributes object.

## RETURN VALUE

If successful, the `pthread_condattr_destroy()` and `pthread_condattr_init()` functions shall return zero; otherwise, an error number shall be returned to indicate the error.

## ERRORS

The `pthread_condattr_init()` function shall fail if:

- **[ENOMEM]**
  - Insufficient memory exists to initialize the condition variable attributes object.

These functions shall not return an error code of **[EINTR]**.

---

*The following sections are informative.*

## EXAMPLES

None.

## APPLICATION USAGE

None.

## RATIONALE

A **process-shared** attribute has been defined for condition variables for the same reason it has been defined for mutexes.

If an implementation detects that the value specified by the `attr` argument to `pthread_condattr_destroy()` does not refer to an initialized condition variable attributes object, it is recommended that the function should fail and report an **[EINVAL]** error.

See also [`pthread_attr_destroy()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_attr_destroy.html) and [`pthread_mutex_destroy()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_mutex_destroy.html).

## FUTURE DIRECTIONS

None.

## SEE ALSO

[`pthread_attr_destroy()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_attr_destroy.html), [`pthread_cond_destroy()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_cond_destroy.html), [`pthread_condattr_getpshared()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_condattr_getpshared.html), [`pthread_create()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_create.html), [`pthread_mutex_destroy()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_mutex_destroy.html)

XBD **[\<pthread.h\>]**](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/pthread.h.html)

## CHANGE HISTORY

First released in Issue 5. Included for alignment with the POSIX Threads Extension.

### Issue 6

The `pthread_condattr_destroy()` and `pthread_condattr_init()` functions are marked as part of the Threads option.

### Issue 7

The `pthread_condattr_destroy()` and `pthread_condattr_init()` functions are moved from the Threads option to the Base.

The **[EINVAL]** error for an uninitialized condition variable attributes object is removed; this condition results in undefined behavior.

