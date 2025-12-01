# memcpy — copy bytes in memory

## SYNOPSIS

```c
#include <string.h>

void *memcpy(void *restrict s1, const void *restrict s2, size_t n);
```

## DESCRIPTION

[CX] The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

The `memcpy()` function shall copy `n` bytes from the object pointed to by `s2` into the object pointed to by `s1`. If copying takes place between objects that overlap, the behavior is undefined.

[CX] The `memcpy()` function shall not change the setting of `errno` on valid input.

## RETURN VALUE

The `memcpy()` function shall return `s1`; no return value is reserved to indicate an error.

## ERRORS

No errors are defined.

---

*The following sections are informative.*

## EXAMPLES

None.

## APPLICATION USAGE

The `memcpy()` function does not check for the overflow of the receiving memory area.

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

XBD [`<string.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/string.h.html)

## CHANGE HISTORY

**First released in Issue 1.** Derived from Issue 1 of the SVID.

### Issue 6

The `memcpy()` prototype is updated for alignment with the ISO/IEC 9899:1999 standard.

### Issue 8

Austin Group Defect 448 is applied, adding a requirement that `memcpy()` does not change the setting of `errno` on valid input.

---

*End of informative text.*