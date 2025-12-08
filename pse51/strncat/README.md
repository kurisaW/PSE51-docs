# strncat — concatenate a string with part of another

## SYNOPSIS

```c
#include <string.h>

char *strncat(char *restrict s1, const char *restrict s2, size_t n);
```

## DESCRIPTION

The `strncat()` function shall append not more than `n` bytes (a NUL character and bytes that follow it are not appended) from the array pointed to by `s2` to the end of the string pointed to by `s1`. The initial byte of `s2` overwrites the NUL character at the end of `s1`. A terminating NUL character is always appended to the result. If copying takes place between objects that overlap, the behavior is undefined.

The `strncat()` function shall not change the setting of `errno` on valid input.

## RETURN VALUE

The `strncat()` function shall return `s1`; no return value shall be reserved to indicate an error.

## ERRORS

No errors are defined.

## EXAMPLES

None.

## APPLICATION USAGE

None.

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- [`strcat()`](strcat.html)
- [`strlcat()`](strlcat.html)

- XBD [`<string.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/string.h.html)

## CHANGE HISTORY

### First released in Issue 1.
Derived from Issue 1 of the SVID.

### Issue 6
The `strncat()` prototype is updated for alignment with the ISO/IEC 9899:1999 standard.

### Issue 8
Austin Group Defect 448 is applied, adding a requirement that `strncat()` does not change the setting of `errno` on valid input.

Austin Group Defect 986 is applied, adding [`strlcat()`](strlcat.html) to the SEE ALSO section.

---
