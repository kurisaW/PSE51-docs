# memchr

## SYNOPSIS

```c
#include <string.h>

void *memchr(const void *s, int c, size_t n);
```

## DESCRIPTION

The `memchr()` function shall locate the first occurrence of `c` (converted to an `unsigned char`) in the initial `n` bytes (each interpreted as `unsigned char`) pointed to by `s`.

The implementation shall behave as if it reads the bytes sequentially and stops as soon as a matching byte is found.

The `memchr()` function shall not change the setting of `errno` on valid input.

## RETURN VALUE

The `memchr()` function shall return a pointer to the located byte, or a null pointer if the byte is not found.

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

`<string.h>`

## CHANGE HISTORY

### First released in Issue 1. Derived from Issue 1 of the SVID.

### Issue 7

POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0374 [110] is applied.

### Issue 8

Austin Group Defect 448 is applied, adding a requirement that `memchr()` does not change the setting of `errno` on valid input.

Austin Group Defect 1302 is applied, aligning this function with the ISO/IEC 9899:2018 standard.

---
