# memcmp — compare bytes in memory

## SYNOPSIS

```c
#include <string.h>

int memcmp(const void *s1, const void *s2, size_t n);
```

## DESCRIPTION

The `memcmp()` function shall compare the first `n` bytes (each interpreted as `unsigned char`) of the object pointed to by `s1` to the first `n` bytes of the object pointed to by `s2`.

The sign of a non-zero return value shall be determined by the sign of the difference between the values of the first pair of bytes (both interpreted as type `unsigned char`) that differ in the objects being compared.

The `memcmp()` function shall not change the setting of `errno` on valid input.

## RETURN VALUE

The `memcmp()` function shall return an integer greater than, equal to, or less than 0, if the object pointed to by `s1` is greater than, equal to, or less than the object pointed to by `s2`, respectively.

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

- `<string.h>`

## CHANGE HISTORY

First released in Issue 1. Derived from Issue 1 of the SVID.

### Issue 8

Austin Group Defect 448 is applied, adding a requirement that `memcmp()` does not change the setting of `errno` on valid input.

---
