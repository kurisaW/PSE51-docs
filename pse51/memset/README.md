# memset

## SYNOPSIS

```c
#include <string.h>

void *memset(void *s, int c, size_t n);
```

## DESCRIPTION

[Option Start] The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard. [Option End]

The `memset()` function shall copy `c` (converted to an **unsigned char**) into each of the first `n` bytes of the object pointed to by `s`.

[Option Start] The `memset()` function shall not change the setting of errno on valid input. [Option End]

## RETURN VALUE

The `memset()` function shall return `s`; no return value is reserved to indicate an error.

## ERRORS

No errors are defined.

* * *

_The following sections are informative._

## EXAMPLES

None.

## APPLICATION USAGE

None.

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

XBD [\<string.h\>](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/string.h.html)

## CHANGE HISTORY

First released in Issue 1. Derived from Issue 1 of the SVID.

### Issue 8

Austin Group Defect 448 is applied, adding a requirement that `memset()` does not change the setting of errno on valid input.

