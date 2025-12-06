# vsnprintf

## SYNOPSIS

```c
#include <stdarg.h>
#include <stdio.h>

/* XSI extension */
int vasprintf(char **restrict ptr, const char *restrict format,
              va_list ap);
int vdprintf(int fildes, const char *restrict format, va_list ap);

int vfprintf(FILE *restrict stream, const char *restrict format,
             va_list ap);
int vprintf(const char *restrict format, va_list ap);
int vsnprintf(char *restrict s, size_t n, const char *restrict format,
              va_list ap);
int vsprintf(char *restrict s, const char *restrict format,
             va_list ap);
```

## DESCRIPTION

The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

The `vasprintf()`, `vdprintf()`, `vfprintf()`, `vprintf()`, `vsnprintf()`, and `vsprintf()` functions shall be equivalent to the `asprintf()`, `dprintf()`, `fprintf()`, `printf()`, `snprintf()`, and `sprintf()` functions respectively, except that instead of being called with a variable number of arguments, they are called with an argument list as defined by `<stdarg.h>`.

These functions shall not invoke the `va_end` macro. As these functions invoke the `va_arg` macro, the value of `ap` after the return is unspecified.

## RETURN VALUE

Refer to `fprintf()`.

## ERRORS

Refer to `fprintf()`.

---

*The following sections are informative.*

## EXAMPLES

None.

## APPLICATION USAGE

Applications using these functions should call `va_end(ap)` afterwards to clean up.

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- 2.5 Standard I/O Streams
- `fprintf()`

XBD `<stdarg.h>`, `<stdio.h>`

## CHANGE HISTORY

First released in Issue 1. Derived from Issue 1 of the SVID.

### Issue 5

The `vsnprintf()` function is added.

### Issue 6

The `vfprintf()`, `vprintf()`, `vsnprintf()`, and `vsprintf()` functions are updated for alignment with the ISO/IEC 9899:1999 standard.

### Issue 7

The `vdprintf()` function is added to complement the `dprintf()` function from The Open Group Technical Standard, 2006, Extended API Set Part 1.

POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0703 [14] is applied.

### Issue 8

Austin Group Defect 1496 is applied, adding the `vasprintf()` function.

---
