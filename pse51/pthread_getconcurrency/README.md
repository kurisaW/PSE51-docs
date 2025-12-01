# pthread_getconcurrency

## Name

pthread_getconcurrency - get the level of concurrency

## Synopsis

```c
#include <pthread.h>

int pthread_getconcurrency(void);
```

## Description

Unbound threads in a process may or may not be required to be simultaneously active. By default, the threads implementation ensures that a sufficient number of threads are active so that the process can continue to make progress. While this conserves system resources, it may not produce the most effective level of concurrency.

The `pthread_setconcurrency()` function allows an application to inform the threads implementation of its desired concurrency level, `new_level`. The actual level of concurrency provided by the implementation as a result of this function call is unspecified.

If `new_level` is zero, it causes the implementation to maintain the concurrency level at its discretion as if `pthread_setconcurrency()` had never been called.

The `pthread_getconcurrency()` function shall return the value set by a previous call to the `pthread_setconcurrency()` function. If the `pthread_setconcurrency()` function was not previously called, this function shall return zero to indicate that the implementation is maintaining the concurrency level.

A call to `pthread_setconcurrency()` shall inform the implementation of its desired concurrency level. The implementation shall use this as a hint, not a requirement.

## Return Value

The `pthread_getconcurrency()` function shall always return the concurrency level set by a previous call to `pthread_setconcurrency()`. If the `pthread_setconcurrency()` function has never been called, `pthread_getconcurrency()` shall return zero.

## Errors

No errors are defined for `pthread_getconcurrency()`.

## Examples

### Example 1: Get Current Concurrency Level

```c
#include <stdio.h>
#include <pthread.h>

int main(void) {
    int level;

    level = pthread_getconcurrency();
    printf("Current concurrency level: %d\n", level);

    if (level == 0) {
        printf("Implementation is maintaining concurrency level\n");
    }

    return 0;
}
```

### Example 2: Set and Get Concurrency Level

```c
#include <stdio.h>
#include <pthread.h>

int main(void) {
    int ret, level;

    // Set desired concurrency level
    ret = pthread_setconcurrency(4);
    if (ret != 0) {
        printf("Failed to set concurrency level\n");
        return 1;
    }

    // Get current concurrency level
    level = pthread_getconcurrency();
    printf("Concurrency level set to: %d\n", level);

    // Reset to implementation-discretion
    ret = pthread_setconcurrency(0);
    if (ret == 0) {
        level = pthread_getconcurrency();
        printf("After reset, concurrency level: %d\n", level);
    }

    return 0;
}
```

## Application Usage

Applications should use `pthread_getconcurrency()` to determine the current concurrency level that was previously set using `pthread_setconcurrency()`. The function is particularly useful for:

- Checking if a specific concurrency level has been set
- Verifying the effect of previous `pthread_setconcurrency()` calls
- Debugging and monitoring thread concurrency behavior

## Rationale

If an implementation does not support multiplexing of user threads on top of several kernel-scheduled entities, the `pthread_setconcurrency()` and `pthread_getconcurrency()` functions are provided for source code compatibility but they shall have no effect when called. To maintain the function semantics, the `new_level` parameter is saved when `pthread_setconcurrency()` is called so that a subsequent call to `pthread_getconcurrency()` shall return the same value.

## Future Directions

None.

## See Also

- `pthread_setconcurrency()`
- `pthread_create()`
- `pthread_attr_init()`

## Copyright

Portions of this text are reprinted and reproduced in electronic form from the IEEE Std 1003.1, 2013 Edition, Standard for Information Technology -- Portable Operating System Interface (POSIX), The Open Group Base Specifications Issue 7, Copyright (C) 2013 by the Institute of Electrical and Electronics Engineers, Inc and The Open Group. In the event of any discrepancy between this version and the original IEEE and The Open Group Standard, the original IEEE and The Open Group Standard shall be the referee. The original Standard can be obtained online at http://www.opengroup.org/unix/online.html.

Any typographical or formatting errors that appear in this page are most likely to have been introduced during the conversion of the source files to man page format. To report such errors, see https://www.kernel.org/doc/man-pages/reporting_bugs.html .