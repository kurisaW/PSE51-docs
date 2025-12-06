# clearerr

## NAME

clearerr — clear indicators on a stream

## SYNOPSIS

```c
#include <stdio.h>

void clearerr(FILE *stream);
```

## DESCRIPTION

The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

The `clearerr()` function shall clear the end-of-file and error indicators for the stream to which `stream` points.

The `clearerr()` function shall not change the setting of `errno` if `stream` is valid.

## RETURN VALUE

The `clearerr()` function shall not return a value.

## ERRORS

No errors are defined.

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

XBD `<stdio.h>`

## CHANGE HISTORY

First released in Issue 1. Derived from Issue 1 of the SVID.

### Issue 7

POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0057 [401] is applied.

---

