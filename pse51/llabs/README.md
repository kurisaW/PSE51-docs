# llabs

The Open Group Base Specifications Issue 8
IEEE Std 1003.1-2024
Copyright © 2001-2024 The IEEE and The Open Group

## NAME

labs, llabs — return a long integer absolute value

## SYNOPSIS

```c
#include <stdlib.h>

long labs(long i);
long long llabs(long long i);
```

## DESCRIPTION

[CX] The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

The `labs()` function shall compute the absolute value of the **long** integer operand `i`. The `llabs()` function shall compute the absolute value of the **long long** integer operand `i`. If the result cannot be represented, the behavior is undefined.

## RETURN VALUE

The `labs()` function shall return the absolute value of the **long** integer operand.

The `llabs()` function shall return the absolute value of the **long long** integer operand.

## ERRORS

No errors are defined.

---

*The following sections are informative.*

## EXAMPLES

None.

## APPLICATION USAGE

Since POSIX.1 requires a two's complement representation of **long** and **long long**, the absolute value of the negative integers with the largest magnitude {LONG_MIN} and {LLONG_MIN} are not representable, thus `labs(LONG_MIN)` and `llabs(LLONG_MIN)` are undefined.

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- [abs()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/abs.html)
- XBD [<stdlib.h>](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdlib.h.html)

## CHANGE HISTORY

### First released in Issue 4. Derived from the ISO C standard.

### Issue 6

The `llabs()` function is added for alignment with the ISO/IEC 9899:1999 standard.

### Issue 7

SD5-XSH-ERN-152 is applied, correcting the RETURN VALUE section.

### Issue 8

Austin Group Defect 1108 is applied, changing the APPLICATION USAGE section.

*End of informative text.*

---

UNIX® is a registered Trademark of The Open Group.
POSIX™ is a Trademark of The IEEE.
Copyright © 2001-2024 The IEEE and The Open Group, All Rights Reserved
[Main Index](https://pubs.opengroup.org/onlinepubs/9799919799/mindex.html) | [XBD](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/contents.html) | [XSH](https://pubs.opengroup.org/onlinepubs/9799919799/functions/contents.html) | [XCU](https://pubs.opengroup.org/onlinepubs/9799919799/utilities/contents.html) | [XRAT](https://pubs.opengroup.org/onlinepubs/9799919799/xrat/contents.html)