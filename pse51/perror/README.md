# perror

The Open Group Base Specifications Issue 8
IEEE Std 1003.1-2024
Copyright © 2001-2024 The IEEE and The Open Group

## NAME

perror — write error messages to standard error

## SYNOPSIS

```c
#include <stdio.h>

void perror(const char *s);
```

## DESCRIPTION

The `perror()` function shall map the error number accessed through the symbol `errno` to a language-dependent error message, which shall be written to the standard error stream as follows:

* First (if `s` is not a null pointer and the character pointed to by `s` is not the null byte), the string pointed to by `s` followed by a `<colon>` and a `<space>`.
* Then an error message string followed by a `<newline>`.

The contents of the error message strings shall be the same as those returned by `strerror()` with argument `errno`.

The `perror()` function shall mark for update the last data modification and last file status change timestamps of the file associated with the standard error stream at some time between its successful completion and `exit()`, `abort()`, or the completion of `fflush()` or `fclose()` on `stderr`.

The `perror()` function shall not change the orientation of the standard error stream.

On error, `perror()` shall set the error indicator for the stream to which `stderr` points, and shall set `errno` to indicate the error.

Since no value is returned, an application wishing to check for error situations should call `clearerr(stderr)` before calling `perror()`, then if `ferror(stderr)` returns non-zero, the value of `errno` indicates which error occurred.

## RETURN VALUE

The `perror()` function shall not return a value.

## ERRORS

Refer to `fputc()`.

---

## EXAMPLES

### Printing an Error Message for a Function

The following example replaces `bufptr` with a buffer that is the necessary size. If an error occurs, the `perror()` function prints a message and the program exits.

```c
#include <stdio.h>
#include <stdlib.h>
...
char *bufptr;
size_t szbuf;
...
if ((bufptr = malloc(szbuf)) == NULL) {
    perror("malloc"); exit(2);
}
...
```

## APPLICATION USAGE

Application writers may prefer to use alternative interfaces instead of `perror()`, such as `strerror_r()` in combination with `fprintf()`.

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- `fprintf()`
- `fputc()`
- `psiginfo()`
- `strerror()`
- `<stdio.h>`

## CHANGE HISTORY

**First released** in Issue 1. Derived from Issue 1 of the SVID.

**Issue 5**: A paragraph is added to the DESCRIPTION indicating that `perror()` does not change the orientation of the standard error stream.

**Issue 6**: Extensions beyond the ISO C standard are marked.

**Issue 7**: SD5-XSH-ERN-95 is applied. Changes are made related to support for finegrained timestamps. POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0429 [389,401], XSH/TC1-2008/0430 [389], and XSH/TC1-2008/0431 [389,401] are applied.

---

*UNIX® is a registered Trademark of The Open Group.
POSIX™ is a Trademark of The IEEE.
Copyright © 2001-2024 The IEEE and The Open Group, All Rights Reserved*