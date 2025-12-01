# asctime — convert date and time to a string

## SYNOPSIS

```c
#include <time.h>

char *asctime(const struct tm *timeptr);
```

## DESCRIPTION

The `asctime()` function shall convert the broken-down time in the structure pointed to by `timeptr` into a string in the form:

```
Sun Sep 16 01:03:52 1973\n\0
```

using the equivalent of the following algorithm:

```c
char *asctime(const struct tm *timeptr)
{
    static char wday_name[7][3] = {
        "Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"
    };
    static char mon_name[12][3] = {
        "Jan", "Feb", "Mar", "Apr", "May", "Jun",
        "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"
    };
    static char result[26];

    sprintf(result, "%.3s %.3s%3d %.2d:%.2d:%.2d %d\n",
        wday_name[timeptr->tm_wday],
        mon_name[timeptr->tm_mon],
        timeptr->tm_mday, timeptr->tm_hour,
        timeptr->tm_min, timeptr->tm_sec,
        1900 + timeptr->tm_year);
    return result;
}
```

If any of the members of the broken-down time contain values that are outside their normal ranges (see XBD `<time.h>`), the behavior of the `asctime()` function is undefined. Likewise, if the calculated year exceeds four digits or is less than the year 1000, the behavior is undefined.

The `tm` structure is defined in the `<time.h>` header.

The `asctime()`, `ctime()`, `gmtime()`, and `localtime()` functions shall return values in one of two static objects: a broken-down time structure and an array of type `char`. Execution of any of the functions that return a pointer to one of these object types may overwrite the information in any object of the same type pointed to by the value returned from any previous call to any of them.

The `asctime()` function need not be thread-safe; however, `asctime()` shall avoid data races with all functions other than itself, `ctime()`, `gmtime()`, and `localtime()`.

## RETURN VALUE

Upon successful completion, `asctime()` shall return a pointer to the string. If the function is unsuccessful, it shall return NULL.

## ERRORS

No errors are defined.

## APPLICATION USAGE

This function is included only for compatibility with older implementations. It has undefined behavior if the resulting string would be too long, so the use of this function should be discouraged. On implementations that do not detect output string length overflow, it is possible to overflow the output buffer in such a way as to cause applications to fail, or possible system security violations. Also, this function does not support localized date and time formats. To avoid these problems, applications should use `strftime()` to generate strings from broken-down times.

Values for the broken-down time structure can be obtained by calling `gmtime()` or `localtime()`.

## RATIONALE

The standard developers decided to mark the `asctime()` function obsolescent even though it is in the ISO C standard due to the possibility of buffer overflow. The ISO C standard also provides the `strftime()` function which can be used to avoid these problems.

## FUTURE DIRECTIONS

This function may be removed in a future version, but not until after it has been removed from the ISO C standard.

## SEE ALSO

- `clock()`
- `ctime()`
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

### First released in Issue 1
Derived from Issue 1 of the SVID.

### Issue 5
- Normative text previously in the APPLICATION USAGE section is moved to the DESCRIPTION.
- The `asctime_r()` function is included for alignment with the POSIX Threads Extension.
- A note indicating that the `asctime()` function need not be reentrant is added to the DESCRIPTION.

### Issue 6
- The `asctime_r()` function is marked as part of the Thread-Safe Functions option.
- Extensions beyond the ISO C standard are marked.
- The APPLICATION USAGE section is updated to include a note on the thread-safe function and its avoidance of possibly using a static data area.
- The DESCRIPTION of `asctime_r()` is updated to describe the format of the string returned.
- The `restrict` keyword is added to the `asctime_r()` prototype for alignment with the ISO/IEC 9899:1999 standard.
- IEEE Std 1003.1-2001/Cor 2-2004, item XSH/TC2/D6/17 is applied, adding the CX extension in the RETURN VALUE section requiring that if the `asctime()` function is unsuccessful it returns NULL.

### Issue 7
- Austin Group Interpretation 1003.1-2001 #053 is applied, marking these functions obsolescent.
- Austin Group Interpretation 1003.1-2001 #156 is applied.
- The `asctime_r()` function is moved from the Thread-Safe Functions option to the Base.
- POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0033 [86,429] is applied.

### Issue 8
- Austin Group Defect 469 is applied, clarifying the conditions under which the behavior of `asctime()` is undefined.
- Austin Group Defect 1302 is applied, aligning this function with the ISO/IEC 9899:2018 standard.
- Austin Group Defect 1330 is applied, changing the FUTURE DIRECTIONS section.
- Austin Group Defect 1376 is applied, removing CX shading from some text derived from the ISO C standard and updating it to match the ISO C standard.
- Austin Group Defect 1410 is applied, removing the `asctime_r()` function.