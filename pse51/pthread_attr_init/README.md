# pthread_attr_init, pthread_attr_destroy - initialize and destroy thread attributes object

## SYNOPSIS

```c
#include <pthread.h>

int pthread_attr_init(pthread_attr_t *attr);
int pthread_attr_destroy(pthread_attr_t *attr);
```

## DESCRIPTION

The `pthread_attr_destroy()` function shall destroy a thread attributes object. An implementation may cause `pthread_attr_destroy()` to set `attr` to an implementation-defined invalid value. A destroyed `attr` attributes object can be reinitialized using `pthread_attr_init()`; the results of otherwise referencing the object after it has been destroyed are undefined.

The `pthread_attr_init()` function shall initialize a thread attributes object `attr` with the default value for all of the individual attributes used by a given implementation.

The resulting attributes object (possibly modified by setting individual attribute values) when used by [`pthread_create()`](pthread_create.html) defines the attributes of the thread created. A single attributes object can be used in multiple simultaneous calls to [`pthread_create()`](pthread_create.html). Results are undefined if `pthread_attr_init()` is called specifying an already initialized `attr` attributes object.

The behavior is undefined if the value specified by the `attr` argument to `pthread_attr_destroy()` does not refer to an initialized thread attributes object.

## RETURN VALUE

Upon successful completion, `pthread_attr_init()` and `pthread_attr_destroy()` shall return zero; otherwise, an error number shall be returned to indicate the error.

## ERRORS

The `pthread_attr_init()` and `pthread_attr_destroy()` functions shall fail if:

- **ENOMEM** - Insufficient memory exists to initialize the thread attributes object.

The `pthread_attr_init()` function may fail if:

- **EBUSY** - The value specified by `attr` refers to an already initialized thread attributes object.

The `pthread_attr_destroy()` function may fail if:

- **EINVAL** - The value specified by `attr` does not refer to an initialized thread attributes object.

These functions shall not return an error code of [`EINTR`](errno.html).

## EXAMPLES

None.

## APPLICATION USAGE

Attributes objects are provided for threads, mutexes, and condition variables as a mechanism to support probable future standardization in these areas without requiring that the function itself be changed.

Attributes objects provide clean isolation of the configurable aspects of threads. For example, "stack size" is an important attribute of a thread, but it cannot be expressed portably. When porting a threaded program, stack sizes often need to be adjusted. The use of attributes objects can help by allowing the changes to be isolated in a single place, rather than being spread across every instance of thread creation.

Attributes objects can be used to set up "classes" of threads with similar attributes; for example, "threads with large stacks and high priority" or "threads with minimal stacks". These classes can be defined in a single place and then referenced wherever threads need to be created. Changes to "class" decisions become straightforward, and detailed analysis of each [`pthread_create()`](pthread_create.html) call is not required.

## RATIONALE

The attributes objects are defined as opaque types as an aid to extensibility. If these objects had been specified as structures, adding new attributes would force recompilation of all multi-threaded programs when the attributes objects are extended; this might not be possible if different program components were supplied by different vendors.

Additionally, opaque attributes objects present opportunities for improving performance. Argument validity can be checked once when attributes are set, rather than each time a thread is created. Implementations often need to cache kernel objects that are expensive to create. Opaque attributes objects provide an efficient mechanism to detect when cached objects become invalid due to attribute changes.

Since assignment is not necessarily defined on a given opaque type, implementation-defined default values cannot be defined in a portable way. The solution to this problem is to allow attributes objects to be initialized dynamically by attributes object initialization functions, so that default values can be supplied automatically by the implementation.

Stack size is defined as an optional attribute because the very notion of a stack is inherently machine-dependent. Some implementations may not be able to change the size of the stack, for example, and others may not need to because stack pages may be discontiguous and can be allocated and released on demand.

The attribute mechanism has been designed in large measure for extensibility. Future extensions to the attribute mechanism or to any attributes object defined in this volume of POSIX.1-2024 has to be done with care so as not to affect binary-compatibility.

Attributes objects, even if allocated by means of dynamic allocation functions such as [`malloc()`](malloc.html), may have their size fixed at compile time. This means, for example, a [`pthread_create()`](pthread_create.html) in an implementation with extensions to `pthread_attr_t` cannot look beyond the area that the binary application assumes is valid. This suggests that implementations should maintain a size field in the attributes object, as well as possibly version information, if extensions in different directions (possibly by different vendors) are to be accommodated.

If an implementation detects that the value specified by the `attr` argument to `pthread_attr_destroy()` does not refer to an initialized thread attributes object, it is recommended that the function should fail and report an `[EINVAL]` error.

If an implementation detects that the value specified by the `attr` argument to `pthread_attr_init()` refers to an already initialized thread attributes object, it is recommended that the function should fail and report an `[EBUSY]` error.

## FUTURE DIRECTIONS

None.

## SEE ALSO

[`pthread_create()`](pthread_create.html), [`malloc()`](malloc.html)

The Base Definitions volume of POSIX.1-2024, `<pthread.h>`

## CHANGE HISTORY

First released in Issue 5. Included in Issue 6 (IEEE Std 1003.1, 2004).

---

*Copyright © 2024 IEEE and The Open Group. All rights reserved.*