# setbuf

## SYNOPSIS

```c
#include <stdio.h>

void setbuf(FILE *restrict stream, char *restrict buf);
```

## DESCRIPTION

[CX] The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

Except that it returns no value, the function call:

```c
setbuf(stream, buf)
```

shall be equivalent to:

```c
setvbuf(stream, buf, _IOFBF, BUFSIZ)
```

if `buf` is not a null pointer, or to:

```c
setvbuf(stream, buf, _IONBF, BUFSIZ)
```

if `buf` is a null pointer.

## RETURN VALUE

The `setbuf()` function shall not return a value.

## ERRORS

Although the `setvbuf()` interface may set `errno` in defined ways, the value of `errno` after a call to `setbuf()` is unspecified.

* * *

*The following sections are informative.*

## EXAMPLES

None.

## APPLICATION USAGE

A common source of error is allocating buffer space as an "automatic" variable in a code block, and then failing to close the stream in the same block.

With `setbuf()`, allocating a buffer of BUFSIZ bytes does not necessarily imply that all of BUFSIZ bytes are used for the buffer area.

Since `errno` is not required to be unchanged on success, in order to correctly detect and possibly recover from errors, applications should use `setvbuf()` instead of `setbuf()`.

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

* [2.5 Standard I/O Streams](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_05)
* [fopen()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fopen.html)
* [setvbuf()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/setvbuf.html)

XBD [<stdio.h>](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdio.h.html)

## CHANGE HISTORY

First released in Issue 1. Derived from Issue 1 of the SVID.

### Issue 6

The prototype for `setbuf()` is updated for alignment with the ISO/IEC 9899:1999 standard.

### Issue 7

POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0546 [397], XSH/TC1-2008/0547 [397], and XSH/TC1-2008/0548 [14] are applied.

