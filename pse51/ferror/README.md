# ferror — test error indicator on a stream

## SYNOPSIS

```c
#include <stdio.h>

int ferror(FILE *stream);
```

## DESCRIPTION

The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

The `ferror()` function shall test the error indicator for the stream pointed to by `stream`.

The `ferror()` function shall not change the setting of `errno` if `stream` is valid.

## RETURN VALUE

The `ferror()` function shall return non-zero if and only if the error indicator is set for `stream`.

## ERRORS

No errors are defined.

---

## EXAMPLES

None.

## APPLICATION USAGE

None.

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- [`clearerr()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/clearerr.html)
- [`feof()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/feof.html)
- [`fopen()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fopen.html)

XBD [`<stdio.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdio.h.html)

## CHANGE HISTORY

First released in Issue 1. Derived from Issue 1 of the SVID.

### Issue 7

POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0125 [401] is applied.

---

*The Open Group Base Specifications Issue 8*
*IEEE Std 1003.1-2024*
*Copyright © 2001-2024 The IEEE and The Open Group*