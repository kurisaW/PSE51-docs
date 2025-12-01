# getchar

The Open Group Base Specifications Issue 8
IEEE Std 1003.1-2024
Copyright © 2001-2024 The IEEE and The Open Group

---

## NAME

getchar — get a byte from a `stdin` stream

## SYNOPSIS

```c
#include <stdio.h>

int getchar(void);
```

## DESCRIPTION

[CX] The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

The `getchar()` function shall be equivalent to `getc(stdin)`.

## RETURN VALUE

Refer to [`fgetc()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fgetc.html).

## ERRORS

Refer to [`fgetc()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fgetc.html).

---

*The following sections are informative.*

## EXAMPLES

None.

## APPLICATION USAGE

If the integer value returned by `getchar()` is stored into a variable of type `char` and then compared against the integer constant EOF, the comparison may never succeed, because sign-extension of a variable of type `char` on widening to integer is implementation-defined.

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- [2.5 Standard I/O Streams](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_05)
- [`getc()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/getc.html)
- XBD [`<stdio.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdio.h.html)

## CHANGE HISTORY

First released in Issue 1. Derived from Issue 1 of the SVID.

### Issue 7

POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0236 [14] is applied.

---

UNIX® is a registered Trademark of The Open Group.
POSIX™ is a Trademark of The IEEE.
Copyright © 2001-2024 The IEEE and The Open Group, All Rights Reserved