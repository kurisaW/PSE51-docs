# asctime_r — convert date and time to a string (REMOVED)

## Status

**REMOVED**: The `asctime_r()` function has been removed from the POSIX.1-2024 standard (Issue 8, Austin Group Defect 1410).

## Historical Information

This page documents the historical context of the `asctime_r()` function which was previously part of the POSIX standard but has been removed.

## NAME (Historical)

> asctime_r — convert date and time to a string (thread-safe)

## SYNOPSIS (Historical)

```c
#include <time.h>

char *asctime_r(const struct tm *restrict timeptr, char *restrict buf);
```

## DESCRIPTION

The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

The `asctime_r()` function was the thread-safe version of `asctime()`, designed to convert the broken-down time in the structure pointed to by `timeptr` into a string in the form:

```
Sun Sep 16 01:03:52 1973\n\0
```

### Key Differences from asctime()

- **Thread Safety**: `asctime_r()` was thread-safe, while `asctime()` was not
- **Buffer Management**: `asctime_r()` required the caller to provide a buffer
- **Static Data Avoidance**: `asctime_r()` avoided using static data areas

## Historical Context

### Evolution Through POSIX Versions

**Issue 5**: The `asctime_r()` function was included for alignment with the POSIX Threads Extension.

**Issue 6**:
- The `asctime_r()` function was marked as part of the Thread-Safe Functions option
- The `restrict` keyword was added to the `asctime_r()` prototype for alignment with the ISO/IEC 9899:1999 standard
- Extensions beyond the ISO C standard were marked
- The APPLICATION USAGE section was updated to include a note on the thread-safe function

**Issue 7**:
- The `asctime_r()` function was moved from the Thread-Safe Functions option to the Base
- Austin Group Interpretation 1003.1-2001 #053 was applied, marking these functions obsolescent

**Issue 8**:
- Austin Group Defect 1410 was applied, **removing the `asctime_r()` function** from the standard

## RATIONALE FOR REMOVAL

The standard developers decided to mark the `asctime()` function (and by extension `asctime_r()`) as obsolescent even though it is in the ISO C standard due to the possibility of buffer overflow. The ISO C standard also provides the `strftime()` function which can be used to avoid these problems.

## APPLICATION USAGE (Historical)

This function was included only for compatibility with older implementations. It had undefined behavior if the resulting string would be too long, so the use of this function should be discouraged.

**Recommended Alternative**: Applications should use `strftime()` to generate strings from broken-down times to avoid buffer overflow problems and to support localized date and time formats.

Values for the broken-down time structure can be obtained by calling `gmtime()` or `localtime()`.

## FUTURE DIRECTIONS

The `asctime_r()` function has been removed and will not be available in future versions of the POSIX standard.

## SEE ALSO

- [`asctime()`](asctime.html) - the non-thread-safe version (also obsolescent)
- [`clock()`](clock.html)
- [`ctime()`](ctime.html)
- [`difftime()`](difftime.html)
- [`gmtime()`](gmtime.html)
- [`localtime()`](localtime.html)
- [`mktime()`](mktime.html)
- [`strftime()`](strftime.html) - **Recommended replacement**
- [`strptime()`](strptime.html)
- [`time()`](time.html)
- XBD [`<time.h>`](../basedefs/time.h.html)

## CHANGE HISTORY

**First released**: Issue 1. Derived from Issue 1 of the SVID.

**Issue 5**:
- Normative text previously in the APPLICATION USAGE section is moved to the DESCRIPTION
- The `asctime_r()` function is included for alignment with the POSIX Threads Extension
- A note indicating that the `asctime()` function need not be reentrant is added to the DESCRIPTION

**Issue 6**:
- The `asctime_r()` function is marked as part of the Thread-Safe Functions option
- Extensions beyond the ISO C standard are marked
- The APPLICATION USAGE section is updated to include a note on the thread-safe function and its avoidance of possibly using a static data area
- The DESCRIPTION of `asctime_r()` is updated to describe the format of the string returned
- The `restrict` keyword is added to the `asctime_r()` prototype for alignment with the ISO/IEC 9899:1999 standard
- IEEE Std 1003.1-2001/Cor 2-2004, item XSH/TC2/D6/17 is applied, adding the CX extension in the RETURN VALUE section requiring that if the `asctime()` function is unsuccessful it returns NULL

**Issue 7**:
- Austin Group Interpretation 1003.1-2001 #053 is applied, marking these functions obsolescent
- Austin Group Interpretation 1003.1-2001 #156 is applied
- The `asctime_r()` function is moved from the Thread-Safe Functions option to the Base
- POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0033 [86,429] is applied

**Issue 8**:
- Austin Group Defect 469 is applied, clarifying the conditions under which the behavior of `asctime()` is undefined
- Austin Group Defect 1302 is applied, aligning this function with the ISO/IEC 9899:2018 standard
- Austin Group Defect 1330 is applied, changing the FUTURE DIRECTIONS section
- Austin Group Defect 1376 is applied, removing CX shading from some text derived from the ISO C standard and updating it to match the ISO C standard
- **Austin Group Defect 1410 is applied, removing the `asctime_r()` function**

---
