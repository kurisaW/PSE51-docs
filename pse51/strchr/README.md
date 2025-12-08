# strchr

## SYNOPSIS

```c
#include <string.h>

char *strchr(const char *s, int c);
```

## DESCRIPTION

[CX] The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

The `strchr()` function shall locate the first occurrence of `c` (converted to a `char`) in the string pointed to by `s`. The terminating NUL character is considered to be part of the string.

[CX] The `strchr()` function shall not change the setting of `errno` on valid input.

## RETURN VALUE

Upon completion, `strchr()` shall return a pointer to the byte, or a null pointer if the byte was not found.

## ERRORS

No errors are defined.

---

*The following sections are informative.*

## EXAMPLES

None.

## APPLICATION USAGE

None.

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- [`strrchr()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/strrchr.html)
- XBD [`<string.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/string.h.html)

## CHANGE HISTORY

First released in Issue 1. Derived from Issue 1 of the SVID.

### Issue 6

Extensions beyond the ISO C standard are marked.

### Issue 8

Austin Group Defect 448 is applied, adding a requirement that `strchr()` does not change the setting of `errno` on valid input.

