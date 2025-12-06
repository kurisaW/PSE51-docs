# getc — get a byte from a stream

## SYNOPSIS

```c
#include <stdio.h>

int getc(FILE *stream);
```

## DESCRIPTION

The `getc()` function shall be equivalent to `fgetc()`, except that if it is implemented as a macro it may evaluate *stream* more than once, so the argument should never be an expression with side-effects.

## RETURN VALUE

Refer to `fgetc()`.

## ERRORS

Refer to `fgetc()`.

---

*The following sections are informative.*

## EXAMPLES

None.

## APPLICATION USAGE

If the integer value returned by `getc()` is stored into a variable of type `char` and then compared against the integer constant EOF, the comparison may never succeed, because sign-extension of a variable of type `char` on widening to integer is implementation-defined.

Since it may be implemented as a macro, `getc()` may treat incorrectly a *stream* argument with side-effects. In particular, `getc(*_f_++)` does not necessarily work as expected. Therefore, use of this function should be preceded by "#undef getc" in such situations; `fgetc()` could also be used.

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- 2.5 Standard I/O Streams
- `fgetc()`
- XBD `<stdio.h>`

## CHANGE HISTORY

First released in Issue 1. Derived from Issue 1 of the SVID.

### Issue 7

POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0231 [14] is applied.

---
