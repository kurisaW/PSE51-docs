# gmtime, gmtime_r — convert a time value to a broken-down UTC time

## SYNOPSIS

```c
#include <time.h>

struct tm *gmtime(const time_t *timer);

[CX] struct tm *gmtime_r(const time_t *restrict timer,
                         struct tm *restrict result);
```

## DESCRIPTION

For `gmtime()`: [CX] The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

The `gmtime()` function shall convert the time in seconds since the Epoch pointed to by `timer` into a broken-down time, expressed as Coordinated Universal Time (UTC).

[CX] The relationship between a time in seconds since the Epoch used as an argument to `gmtime()` and the `tm` structure (defined in the `<time.h>` header) is that the result shall be as specified in the expression given in the definition of seconds since the Epoch (see XBD 4.19 Seconds Since the Epoch), where the names in the structure and in the expression correspond.

The same relationship shall apply for `gmtime_r()`.

The `gmtime()` function need not be thread-safe; however, `gmtime()` shall avoid data races with all functions other than itself, `asctime()`, `ctime()`, and `localtime()`.

The `asctime()`, `ctime()`, `gmtime()`, and `localtime()` functions shall return values in one of two static objects: a broken-down time structure and an array of type `char`. Execution of any of the functions that return a pointer to one of these object types may overwrite the information in any object of the same type pointed to by the value returned from any previous call to any of them.

[CX] The `gmtime_r()` function shall convert the time in seconds since the Epoch pointed to by `timer` into a broken-down time expressed as Coordinated Universal Time (UTC). The broken-down time is stored in the structure referred to by `result`. The `gmtime_r()` function shall also return the address of the same structure.

## RETURN VALUE

Upon successful completion, the `gmtime()` function shall return a pointer to a `struct tm`. If an error is detected, `gmtime()` shall return a null pointer [CX] and set `errno` to indicate the error.

Upon successful completion, `gmtime_r()` shall return the address of the structure pointed to by the argument `result`. The structure's `tm_zone` member shall be set to a pointer to the string "UTC", which shall have static storage duration. If an error is detected, `gmtime_r()` shall return a null pointer and set `errno` to indicate the error.

## ERRORS

The `gmtime()` [CX] and `gmtime_r()` functions shall fail if:

- **[EOVERFLOW]** [CX] The result cannot be represented.

## EXAMPLES

None.

## APPLICATION USAGE

The `gmtime_r()` function is thread-safe and returns values in a user-supplied buffer instead of possibly using a static data area that may be overwritten by each call.

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

`asctime()`, `clock()`, `ctime()`, `difftime()`, `futimens()`, `localtime()`, `mktime()`, `strftime()`, `strptime()`, `time()`

XBD 4.19 Seconds Since the Epoch, `<time.h>`

## CHANGE HISTORY

### First released in Issue 1. Derived from Issue 1 of the SVID.

### Issue 5

A note indicating that the `gmtime()` function need not be reentrant is added to the DESCRIPTION.

The `gmtime_r()` function is included for alignment with the POSIX Threads Extension.

### Issue 6

The `gmtime_r()` function is marked as part of the Thread-Safe Functions option.

Extensions beyond the ISO C standard are marked.

The APPLICATION USAGE section is updated to include a note on the thread-safe function and its avoidance of possibly using a static data area.

The `restrict` keyword is added to the `gmtime_r()` prototype for alignment with the ISO/IEC 9899:1999 standard.

IEEE Std 1003.1-2001/Cor 1-2002, item XSH/TC1/D6/27 is applied, adding the [EOVERFLOW] error.

IEEE Std 1003.1-2001/Cor 2-2004, item XSH/TC2/D6/48 is applied, updating the error handling for `gmtime_r()`.

### Issue 7

Austin Group Interpretation 1003.1-2001 #156 is applied.

The `gmtime_r()` function is moved from the Thread-Safe Functions option to the Base.

### Issue 8

Austin Group Defect 1302 is applied, aligning the `gmtime()` function with the ISO/IEC 9899:2018 standard.

Austin Group Defect 1376 is applied, removing CX shading from some text derived from the ISO C standard and updating it to match the ISO C standard.

Austin Group Defect 1533 is applied, adding `tm_gmtoff` and `tm_zone` to the `tm` structure.