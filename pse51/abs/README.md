# abs

## SYNOPSIS

```c
#include <stdlib.h>

int abs(int i);
```

## DESCRIPTION

[CX] The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

The `abs()` function shall compute the absolute value of its integer operand, `i`. If the result cannot be represented, the behavior is undefined.

## RETURN VALUE

The `abs()` function shall return the absolute value of its integer operand.

## ERRORS

No errors are defined.

* * *

*The following sections are informative.*

## EXAMPLES

None.

## APPLICATION USAGE

Since POSIX.1 requires a two's complement representation of `int`, the absolute value of the negative integer with the largest magnitude {INT_MIN} is not representable, thus `abs(INT_MIN)` is undefined.

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- [`fabs()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fabs.html)
- [`labs()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/labs.html)

XBD [`<stdlib.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdlib.h.html)

## CHANGE HISTORY

First released in Issue 1. Derived from Issue 1 of the SVID.

### Issue 6

Extensions beyond the ISO C standard are marked.

### Issue 8

Austin Group Defect 1108 is applied, changing the APPLICATION USAGE section.

