# putchar

The Open Group Base Specifications Issue 8
IEEE Std 1003.1-2024
Copyright © 2001-2024 The IEEE and The Open Group

## NAME

putchar — put a byte on a stdout stream

## SYNOPSIS

```c
#include <stdio.h>

int putchar(int c);
```

## DESCRIPTION

[CX] The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

The function call `putchar(c)` shall be equivalent to `putc(c,stdout)`.

## RETURN VALUE

Refer to `fputc()`.

## ERRORS

Refer to `fputc()`.

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

- 2.5 Standard I/O Streams
- `putc()`
- XBD `<stdio.h>`

## CHANGE HISTORY

First released in Issue 1. Derived from Issue 1 of the SVID.

### Issue 7

POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0471 [14] is applied.

---

