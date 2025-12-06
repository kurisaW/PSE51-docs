# strcat

## SYNOPSIS

```c
#include <string.h>

char *strcat(char *restrict s1, const char *restrict s2);
```

## DESCRIPTION

The `strcat()` function shall append a copy of the string pointed to by `s2` (including the terminating NUL character) to the end of the string pointed to by `s1`. The initial byte of `s2` overwrites the NUL character at the end of `s1`. If copying takes place between objects that overlap, the behavior is undefined.

The `strcat()` function shall not change the setting of `errno` on valid input.

## RETURN VALUE

The `strcat()` function shall return `s1`; no return value is reserved to indicate an error.

## ERRORS

No errors are defined.

---

*The following sections are informative.*

## EXAMPLES

None.

## APPLICATION USAGE

This version is aligned with the ISO C standard; this does not affect compatibility with XPG3 applications. Reliable error detection by this function was never guaranteed.

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- [`strncat()`](strncat.html)
- XBD [`<string.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/string.h.html)

## CHANGE HISTORY

First released in Issue 1. Derived from Issue 1 of the SVID.

### Issue 6

The `strcat()` prototype is updated for alignment with the ISO/IEC 9899:1999 standard.

### Issue 8

Austin Group Defect 448 is applied, adding a requirement that `strcat()` does not change the setting of `errno` on valid input.

---
