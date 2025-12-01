# gets

The Open Group Base Specifications Issue 7, 2018 edition
IEEE Std 1003.1-2017 (Revision of IEEE Std 1003.1-2008)
Copyright © 2001-2018 IEEE and The Open Group

A newer edition of this document exists [here](http://pubs.opengroup.org/onlinepubs/9799919799/)

---

## NAME

gets - get a string from a stdin stream

## SYNOPSIS

```c
#include <stdio.h>

char *gets(char *s);
```

## DESCRIPTION

The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2017 defers to the ISO C standard.

The `gets()` function shall read bytes from the standard input stream, `stdin`, into the array pointed to by `s`, until a `<newline>` is read or an end-of-file condition is encountered. Any `<newline>` shall be discarded and a null byte shall be placed immediately after the last byte read into the array.

The `gets()` function may mark the last data access timestamp of the file associated with `stream` for update. The last data access timestamp shall be marked for update by the first successful execution of `fgetc()`, `fgets()`, `fread()`, `fscanf()`, `getc()`, `getchar()`, `getdelim()`, `getline()`, `gets()`, or `scanf()` using `stream` that returns data not supplied by a prior call to `ungetc()`.

## RETURN VALUE

Upon successful completion, `gets()` shall return `s`. If the end-of-file indicator for the stream is set, or if the stream is at end-of-file, the end-of-file indicator for the stream shall be set and `gets()` shall return a null pointer. If a read error occurs, the error indicator for the stream shall be set, `gets()` shall return a null pointer, and set `errno` to indicate the error.

## ERRORS

Refer to [`fgetc()`](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fgetc.html).

---

*The following sections are informative.*

## EXAMPLES

None.

## APPLICATION USAGE

Reading a line that overflows the array pointed to by `s` results in undefined behavior. The use of [`fgets()`](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fgets.html) is recommended.

Since the user cannot specify the length of the buffer passed to `gets()`, use of this function is discouraged. The length of the string read is unlimited. It is possible to overflow this buffer in such a way as to cause applications to fail, or possible system security violations.

Applications should use the [`fgets()`](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fgets.html) function instead of the obsolescent `gets()` function.

## RATIONALE

The standard developers decided to mark the `gets()` function as obsolescent even though it is in the ISO C standard due to the possibility of buffer overflow.

## FUTURE DIRECTIONS

The `gets()` function may be removed in a future version.

## SEE ALSO

- [Standard I/O Streams](https://pubs.opengroup.org/onlinepubs/9699919799/functions/V2_chap02.html#tag_15_05)
- [`feof()`](https://pubs.opengroup.org/onlinepubs/9699919799/functions/feof.html)
- [`ferror()`](https://pubs.opengroup.org/onlinepubs/9699919799/functions/ferror.html)
- [`fgets()`](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fgets.html)
- [<stdio.h>](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/stdio.h.html)

## CHANGE HISTORY

First released in Issue 1. Derived from Issue 1 of the SVID.

### Issue 6

Extensions beyond the ISO C standard are marked.

### Issue 7

Austin Group Interpretation 1003.1-2001 #051 is applied, clarifying the RETURN VALUE section.

The `gets()` function is marked obsolescent.

Changes are made related to support for finegrained timestamps.

POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0257 [14] is applied.

*End of informative text.*

---

UNIX ® is a registered Trademark of The Open Group.
POSIX ™ is a Trademark of The IEEE.
Copyright © 2001-2018 IEEE and The Open Group, All Rights Reserved