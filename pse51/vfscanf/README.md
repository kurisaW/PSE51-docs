# vfscanf, vscanf, vsscanf — format input of a stdarg argument list

## SYNOPSIS

```c
#include <stdarg.h>
#include <stdio.h>

int vfscanf(FILE *restrict stream,
           const char *restrict format,
           va_list arg);

int vscanf(const char *restrict format,
           va_list arg);

int vsscanf(const char *restrict s,
            const char *restrict format,
            va_list arg);
```

## DESCRIPTION

[CX] The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

The `vscanf()`, `vfscanf()`, and `vsscanf()` functions shall be equivalent to the [`scanf()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/scanf.html), [`fscanf()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fscanf.html), and [`sscanf()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sscanf.html) functions, respectively, except that instead of being called with a variable number of arguments, they are called with an argument list as defined in the [`<stdarg.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdarg.h.html) header. These functions shall not invoke the `va_end` macro. As these functions invoke the `va_arg` macro, the value of `ap` after the return is unspecified.

## RETURN VALUE

Refer to [`fscanf()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fscanf.html).

## ERRORS

Refer to [`fscanf()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fscanf.html).

---

## EXAMPLES

None.

## APPLICATION USAGE

Applications using these functions should call `va_end(ap)` afterwards to clean up.

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- [2.5 Standard I/O Streams](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_05)
- [`fscanf()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fscanf.html)
- XBD [`<stdarg.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdarg.h.html), [`<stdio.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdio.h.html)

## CHANGE HISTORY

First released in Issue 6. Derived from the ISO/IEC 9899:1999 standard.

### Issue 7

POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0704 [14] is applied.

---

*The Open Group Base Specifications Issue 8*
*IEEE Std 1003.1-2024*
*Copyright © 2001-2024 The IEEE and The Open Group*