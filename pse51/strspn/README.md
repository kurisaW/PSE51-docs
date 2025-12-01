# strspn — get length of a substring

## SYNOPSIS

```c
#include <string.h>

size_t strspn(const char *s1, const char *s2);
```

## DESCRIPTION

The `strspn()` function shall compute the length (in bytes) of the maximum initial segment of the string pointed to by `s1` which consists entirely of bytes from the string pointed to by `s2`.

The `strspn()` function shall not change the setting of `errno` on valid input.

## RETURN VALUE

The `strspn()` function shall return the computed length; no return value is reserved to indicate an error.

## ERRORS

No errors are defined.

## APPLICATION USAGE

None.

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- `strcspn()`
- `<string.h>`

## CHANGE HISTORY

**First released in Issue 1.** Derived from Issue 1 of the SVID.

**Issue 5:** The RETURN VALUE section is updated to indicate that `strspn()` returns the length of `s`, and not `s` itself as was previously stated.

**Issue 7:** SD5-XSH-ERN-182 is applied.

**Issue 8:** Austin Group Defect 448 is applied, adding a requirement that `strspn()` does not change the setting of `errno` on valid input.