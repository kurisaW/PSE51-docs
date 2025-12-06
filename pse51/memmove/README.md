# memmove

## SYNOPSIS

```c
#include <string.h>

void *memmove(void *s1, const void *s2, size_t n);
```

## DESCRIPTION

[CX] The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

The `memmove()` function shall copy `n` bytes from the object pointed to by `s2` into the object pointed to by `s1`. Copying takes place as if the `n` bytes from the object pointed to by `s2` are first copied into a temporary array of `n` bytes that does not overlap the objects pointed to by `s1` and `s2`, and then the `n` bytes from the temporary array are copied into the object pointed to by `s1`.

[CX] The `memmove()` function shall not change the setting of `errno` on valid input.

## RETURN VALUE

The `memmove()` function shall return `s1`; no return value is reserved to indicate an error.

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

XBD `<string.h>`

## CHANGE HISTORY

First released in Issue 4. Derived from the ANSI C standard.

### Issue 8

Austin Group Defect 448 is applied, adding a requirement that `memmove()` does not change the setting of `errno` on valid input.

---
