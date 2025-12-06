# va_end

## SYNOPSIS

```c
#include <stdarg.h>

void va_end(va_list ap);
```

## DESCRIPTION

The `va_end()` macro is used to clean up after traversal of a variable argument list. It invalidates the `va_list` object `ap` for use (unless `va_start()` or `va_copy()` is invoked again).

The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

## REQUIREMENTS

Each invocation of the `va_start()` and `va_copy()` macros shall be matched by a corresponding invocation of the `va_end()` macro in the same function.

## USAGE RULES

- The object `ap` may be passed as an argument to another function
- If that function invokes the `va_arg()` macro with parameter `ap`, the value of `ap` in the calling function is unspecified and shall be passed to the `va_end()` macro prior to any further reference to `ap`
- Neither the `va_copy()` nor `va_start()` macro shall be invoked to reinitialize `ap` without an intervening invocation of the `va_end()` macro for the same `ap`

## EXAMPLE

```c
#include <stdarg.h>
#include <unistd.h>
#include <stdio.h>

#define MAXARGS 31

/*
 * execl is called by
 * execl(file, arg1, arg2, ..., (char *)(0));
 */
int execl(const char *file, const char *args, ...)
{
    va_list ap;
    char *array[MAXARGS + 1];
    int argno = 0;

    va_start(ap, args);
    while (args != 0 && argno < MAXARGS)
    {
        array[argno++]] = args;
        args = va_arg(ap, const char *);
    }
    array[argno] = (char *) 0;
    va_end(ap);
    return execv(file, array);
}
```

## RELATED FUNCTIONS

- `va_start()` - Initialize a variable argument list
- `va_arg()` - Retrieve the next argument from a variable argument list
- `va_copy()` - Copy a variable argument list state

## SEE ALSO

- [`printf()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/printf.html)
- [`execl()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/execl.html)

---

*Source: The Open Group Base Specifications Issue 8, IEEE Std 1003.1-2024*