# ctime — convert a time value to a date and time string

## SYNOPSIS

```c
#include <time.h>

char *ctime(const time_t *clock);
```

## DESCRIPTION

The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

The `ctime()` function shall convert the time pointed to by `clock`, representing time in seconds since the Epoch, to local time in the form of a string. It shall be equivalent to:

```c
asctime(localtime(clock))
```

The `asctime()`, `ctime()`, `gmtime()`, and `localtime()` functions shall return values in one of two static objects: a broken-down time structure and an array of `char`. Execution of any of the functions that return a pointer to one of these object types may overwrite the information in any object of the same type pointed to by the value returned from any previous call to any of them.

The `ctime()` function need not be thread-safe; however, `ctime()` shall avoid data races with all functions other than itself, `asctime()`, `gmtime()`, and `localtime()`.

## RETURN VALUE

The `ctime()` function shall return the pointer returned by `asctime()` with that broken-down time as an argument.

## ERRORS

No errors are defined.

---

## EXAMPLES

None.

## APPLICATION USAGE

This function is included only for compatibility with older implementations. It has undefined behavior if the resulting string would be too long, so the use of this function should be discouraged. On implementations that do not detect output string length overflow, it is possible to overflow the output buffer in such a way as to cause applications to fail, or possible system security violations. Also, this function does not support localized date and time formats. To avoid these problems, applications should use `strftime()` to generate strings from broken-down times.

Values for the broken-down time structure can be obtained by calling `gmtime()` or `localtime()`.

Attempts to use `ctime()` for times before the Epoch or for times beyond the year 9999 produce undefined results. Refer to `asctime()`.

## RATIONALE

The standard developers decided to mark the `ctime()` function obsolescent even though it is in the ISO C standard due to the possibility of buffer overflow. The ISO C standard also provides the `strftime()` function which can be used to avoid these problems.

## FUTURE DIRECTIONS

This function may be removed in a future version, but not until after it has been removed from the ISO C standard.

## SEE ALSO

- `asctime()`
- `clock()`
- `difftime()`
- `futimens()`
- `gmtime()`
- `localtime()`
- `mktime()`
- `strftime()`
- `strptime()`
- `time()`
- XBD `<time.h>`

## CHANGE HISTORY

### First released in Issue 1. Derived from Issue 1 of the SVID.

### Issue 5

- Normative text previously in the APPLICATION USAGE section is moved to the DESCRIPTION.
- The `ctime_r()` function is included for alignment with the POSIX Threads Extension.
- A note indicating that the `ctime()` function need not be reentrant is added to the DESCRIPTION.

### Issue 6

- Extensions beyond the ISO C standard are marked.
- In the DESCRIPTION, the note about reentrancy is expanded to cover thread-safety.
- The APPLICATION USAGE section is updated to include a note on the thread-safe function and its avoidance of possibly using a static data area.

### Issue 7

- Austin Group Interpretation 1003.1-2001 #156 is applied.
- SD5-XSH-ERN-25 is applied, updating the APPLICATION USAGE.
- Austin Group Interpretation 1003.1-2001 #053 is applied, marking these functions obsolescent.
- The `ctime_r()` function is moved from the Thread-Safe Functions option to the Base.
- POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0066 [321,428] is applied.
- POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0075 [664] is applied.

### Issue 8

- Austin Group Defect 1302 is applied, aligning this function with the ISO/IEC 9899:2018 standard.
- Austin Group Defect 1330 is applied, changing the FUTURE DIRECTIONS section.
- Austin Group Defect 1376 is applied, removing CX shading from some text derived from the ISO C standard and updating it to match the ISO C standard.
- Austin Group Defect 1410 is applied, removing the `ctime_r()` function.

---
