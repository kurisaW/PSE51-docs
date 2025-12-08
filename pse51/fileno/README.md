# fileno — map a stream pointer to a file descriptor

## SYNOPSIS

```c
#include <stdio.h>

int fileno(FILE *stream);
```

## DESCRIPTION

The `fileno()` function shall return the integer file descriptor associated with the stream pointed to by `stream`.

## RETURN VALUE

Upon successful completion, `fileno()` shall return the integer value of the file descriptor associated with `stream`. Otherwise, the value -1 shall be returned and `errno` set to indicate the error.

## ERRORS

The `fileno()` function shall fail if:

- **[EBADF]**
  The stream is not associated with a file.

The `fileno()` function may fail if:

- **[EBADF]**
  The file descriptor underlying `stream` is not a valid file descriptor.

## EXAMPLES

None.

## APPLICATION USAGE

None.

## RATIONALE

Without some specification of which file descriptors are associated with these streams, it is impossible for an application to set up the streams for another application it starts with `fork()` and `exec`. In particular, it would not be possible to write a portable version of the `sh` command interpreter (although there may be other constraints that would prevent that portability).

## FUTURE DIRECTIONS

None.

## SEE ALSO

- *2.5.1 Interaction of File Descriptors and Standard I/O Streams*
- `dirfd()`
- `fdopen()`
- `fopen()`
- `stdin`
- **XBD** `<stdio.h>`

## CHANGE HISTORY

First released in Issue 1. Derived from Issue 1 of the SVID.

### Issue 6

The following new requirements on POSIX implementations derive from alignment with the Single UNIX Specification:

- The [EBADF] optional error condition is added.

### Issue 7

SD5-XBD-ERN-99 is applied, changing the definition of the [EBADF] error.

POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0115 [589] is applied.

---
