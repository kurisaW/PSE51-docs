# puts

## SYNOPSIS

```c
#include <stdio.h>

int puts(const char *s);
```

## DESCRIPTION

The `puts()` function shall write the string pointed to by `s`, followed by a newline, to the standard output stream `stdout`. The terminating null byte shall not be written.

The last data modification and last file status change timestamps of the file shall be marked for update between the successful execution of `puts()` and the next successful completion of a call to `fflush()` or `fclose()` on the same stream or a call to `exit()` or `abort()`.

## RETURN VALUE

Upon successful completion, `puts()` shall return a non-negative number. Otherwise, it shall return EOF, shall set an error indicator for the stream, and `errno` shall be set to indicate the error.

## ERRORS

Refer to `fputc()`.

---

*The following sections are informative.*

## EXAMPLES

### Printing to Standard Output

The following example gets the current time, converts it to a string using `localtime()` and `asctime()`, and prints it to standard output using `puts()`. It then prints the number of minutes to an event for which it is waiting.

```c
#include <time.h>
#include <stdio.h>
...
time_t now;
int minutes_to_event;
...
time(&now);
printf("The time is ");
puts(asctime(localtime(&now)));
printf("There are %d minutes to the event.\n",
    minutes_to_event);
...
```

## APPLICATION USAGE

The `puts()` function appends a newline, while `fputs()` does not.

This volume of POSIX.1-2024 requires that successful completion simply return a non-negative integer. There are at least three known different implementation conventions for this requirement:

* Return a constant value.
* Return the last character written.
* Return the number of bytes written. Note that this implementation convention cannot be adhered to for strings longer than {INT_MAX} bytes as the value would not be representable in the return type of the function. For backwards compatibility, implementations can return the number of bytes for strings of up to {INT_MAX} bytes, and return {INT_MAX} for all longer strings.

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

[2.5 Standard I/O Streams](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_05), [fopen()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fopen.html), [fputs()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fputs.html), [putc()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/putc.html)

XBD [<stdio.h>](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdio.h.html)

## CHANGE HISTORY

First released in Issue 1. Derived from Issue 1 of the SVID.

### Issue 6

Extensions beyond the ISO C standard are marked.

### Issue 7

Changes are made related to support for finegrained timestamps.

POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0476 [174,412] and XSH/TC1-2008/0477 [14] are applied.

