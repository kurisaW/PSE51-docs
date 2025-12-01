# putc — put a byte on a stream

## SYNOPSIS

```c
#include <stdio.h>

int putc(int c, FILE *stream);
```

## DESCRIPTION

The `putc()` function shall be equivalent to `fputc()`, except that if it is implemented as a macro it may evaluate `stream` more than once, so the argument should never be an expression with side-effects.

## RETURN VALUE

Refer to `fputc()`.

## ERRORS

Refer to `fputc()`.

## EXAMPLES

None.

## APPLICATION USAGE

Since it may be implemented as a macro, `putc()` may treat a `stream` argument with side-effects incorrectly. In particular, `putc(c,*f++)` does not necessarily work correctly. Therefore, use of this function is not recommended in such situations; `fputc()` should be used instead.

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

2.5 Standard I/O Streams, `fputc()`

XBD `<stdio.h>`

## CHANGE HISTORY

First released in Issue 1. Derived from Issue 1 of the SVID.

### Issue 7

POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0470 [14] is applied.

---

*Source: The Open Group Base Specifications Issue 8, IEEE Std 1003.1-2024*