# strpbrk

## SYNOPSIS

```c
#include <string.h>

char *strpbrk(const char *s1, const char *s2);
```

## DESCRIPTION

[CX] The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

The `strpbrk()` function shall locate the first occurrence in the string pointed to by `s1` of any byte from the string pointed to by `s2`.

[CX] The `strpbrk()` function shall not change the setting of `errno` on valid input.

## RETURN VALUE

Upon successful completion, `strpbrk()` shall return a pointer to the byte or a null pointer if no byte from `s2` occurs in `s1`.

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

- [`strchr()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/strchr.html)
- [`strrchr()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/strrchr.html)
- XBD [`<string.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/string.h.html)

## CHANGE HISTORY

First released in Issue 1. Derived from Issue 1 of the SVID.

### Issue 8

Austin Group Defect 448 is applied, adding a requirement that `strpbrk()` does not change the setting of `errno` on valid input.

