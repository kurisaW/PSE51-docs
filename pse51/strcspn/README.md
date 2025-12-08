# strcspn

## SYNOPSIS

```c
#include <string.h>

size_t strcspn(const char *s1, const char *s2);
```

## DESCRIPTION

The `strcspn()` function shall compute the length (in bytes) of the maximum initial segment of the string pointed to by `s1` which consists entirely of bytes **not** from the string pointed to by `s2`.

The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

The `strcspn()` function shall not change the setting of `errno` on valid input.

## RETURN VALUE

The `strcspn()` function shall return the length of the computed segment of the string pointed to by `s1`; no return value is reserved to indicate an error.

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

- [`strspn()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/strspn.html)
- XBD [`<string.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/string.h.html)

## CHANGE HISTORY

**First released in Issue 1.** Derived from Issue 1 of the SVID.

### Issue 5

The RETURN VALUE section is updated to indicate that `strcspn()` returns the length of `s1`, and not `s1` itself as was previously stated.

### Issue 6

The Open Group Corrigendum U030/1 is applied. The text of the RETURN VALUE section is updated to indicate that the computed segment length is returned, not the `s1` length.

### Issue 8

Austin Group Defect 448 is applied, adding a requirement that `strcspn()` does not change the setting of `errno` on valid input.

---
