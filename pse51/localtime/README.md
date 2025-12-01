# localtime, localtime_r — convert a time value to a broken-down local time

## SYNOPSIS

```c
#include <time.h>

struct tm *localtime(const time_t *timer);

[CX] struct tm *localtime_r(const time_t *restrict timer,
                           struct tm *restrict result);
```

## DESCRIPTION

For `localtime()`:
[CX] The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

The `localtime()` function shall convert the time in seconds since the Epoch pointed to by `timer` into a broken-down time, expressed as a local time. The function corrects for the timezone and any seasonal time adjustments. [CX] Local timezone information shall be set as though `localtime()` calls `tzset()`.

The relationship between a time in seconds since the Epoch used as an argument to `localtime()` and the `tm` structure (defined in the `<time.h>` header) is that the result shall be as specified in the expression given in the definition of seconds since the Epoch (see XBD 4.19 Seconds Since the Epoch) corrected for timezone and any seasonal time adjustments, where the names in the structure and in the expression correspond.

The same relationship shall apply for `localtime_r()`.

The `localtime()` function need not be thread-safe; however, `localtime()` shall avoid data races with all functions other than itself, `asctime()`, `ctime()`, and `gmtime()`.

The `asctime()`, `ctime()`, `gmtime()`, and `localtime()` functions shall return values in one of two static objects: a broken-down time structure and an array of type `char`. Execution of any of the functions that return a pointer to one of these object types may overwrite the information in any object of the same type pointed to by the value returned from any previous call to any of them.

[CX] The `localtime_r()` function shall convert the time in seconds since the Epoch pointed to by `timer` into a broken-down time stored in the structure to which `result` points. The `localtime_r()` function shall also return a pointer to that same structure.

Unlike `localtime()`, the `localtime_r()` function is not required to set `tzname`. If `localtime_r()` sets `tzname`, it shall also set `daylight` and `timezone`. If `localtime_r()` does not set `tzname`, it shall not set `daylight` and shall not set `timezone`. If the `tm` structure member `tm_zone` is accessed after the value of `TZ` is subsequently modified, the behaviour is undefined.

## RETURN VALUE

Upon successful completion, the `localtime()` function shall return a pointer to the broken-down time structure. If an error is detected, `localtime()` shall return a null pointer [CX] and set `errno` to indicate the error.

Upon successful completion, `localtime_r()` shall return a pointer to the structure pointed to by the argument `result`. If an error is detected, `localtime_r()` shall return a null pointer and set `errno` to indicate the error.

## ERRORS

The `localtime()` [CX] and `localtime_r()` functions shall fail if:

- **[EOVERFLOW]**: [CX] The result cannot be represented.

## EXAMPLES

### Getting the Local Date and Time

The following example uses the `time()` function to calculate the time elapsed, in seconds, since January 1, 1970 0:00 UTC (the Epoch), `localtime()` to convert that value to a broken-down time, and `asctime()` to convert the broken-down time values into a printable string.

```c
#include <stdio.h>
#include <time.h>

int main(void)
{
    time_t result;

    result = time(NULL);
    printf("%s%ju secs since the Epoch\n",
        asctime(localtime(&result)),
            (uintmax_t)result);
    return(0);
}
```

This example writes the current time to `stdout` in a form like this:

```
Wed Jun 26 10:32:15 1996
835810335 secs since the Epoch
```

### Getting the Modification Time for a File

The following example prints the last data modification timestamp in the local timezone for a given file.

```c
#include <stdio.h>
#include <time.h>
#include <sys/stat.h>

int
print_file_time(const char *pathname)
{
    struct stat statbuf;
    struct tm *tm;
    char timestr[BUFSIZ];

    if(stat(pathname, &statbuf) == -1)
        return -1;
    if((tm = localtime(&statbuf.st_mtime)) == NULL)
        return -1;
    if(strftime(timestr, sizeof(timestr), "%Y-%m-%d %H:%M:%S", tm) == 0)
        return -1;
    printf("%s: %s.%09ld\n", pathname, timestr, statbuf.st_mtim.tv_nsec);
    return 0;
}
```

### Timing an Event

The following example gets the current time, converts it to a string using `localtime()` and `asctime()`, and prints it to standard output using `fputs()`. It then prints the number of minutes to an event being timed.

```c
#include <time.h>
#include <stdio.h>
...
time_t now;
int minutes_to_event;
...
time(&now);
printf("The time is ");
fputs(asctime(localtime(&now)), stdout);
printf("There are still %d minutes to the event.\n",
    minutes_to_event);
...
```

## APPLICATION USAGE

The `localtime_r()` function is thread-safe and returns values in a user-supplied buffer instead of possibly using a static data area that may be overwritten by each call.

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

`asctime()`, `clock()`, `ctime()`, `difftime()`, `futimens()`, `getdate()`, `gmtime()`, `mktime()`, `strftime()`, `strptime()`, `time()`, `tzset()`

XBD 4.19 Seconds Since the Epoch, `<time.h>`

## CHANGE HISTORY

First released in Issue 1. Derived from Issue 1 of the SVID.

### Issue 5

A note indicating that the `localtime()` function need not be reentrant is added to the DESCRIPTION.

The `localtime_r()` function is included for alignment with the POSIX Threads Extension.

### Issue 6

The `localtime_r()` function is marked as part of the Thread-Safe Functions option.

Extensions beyond the ISO C standard are marked.

The APPLICATION USAGE section is updated to include a note on the thread-safe function and its avoidance of possibly using a static data area.

The `restrict` keyword is added to the `localtime_r()` prototype for alignment with the ISO/IEC 9899:1999 standard.

Examples are added.

IEEE Std 1003.1-2001/Cor 1-2002, item XSH/TC1/D6/32 is applied, adding the [EOVERFLOW] error.

IEEE Std 1003.1-2001/Cor 2-2004, item XSH/TC2/D6/55 is applied, updating the error handling for `localtime_r()`.

IEEE Std 1003.1-2001/Cor 2-2004, item XSH/TC2/D6/56 is applied, adding a requirement that if `localtime_r()` does not set the `tzname` variable, it shall not set the `daylight` or `timezone` variables. On systems supporting XSI, the `daylight`, `timezone`, and `tzname` variables should all be set to provide information for the same timezone. This updates the description of `localtime_r()` to mention `daylight` and `timezone` as well as `tzname`. The SEE ALSO section is updated.

### Issue 7

Austin Group Interpretation 1003.1-2001 #156 is applied.

The `localtime_r()` function is moved from the Thread-Safe Functions option to the Base.

Changes are made to the EXAMPLES section related to support for finegrained timestamps.

POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0363 [291] is applied.

POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0201 [664] is applied.

### Issue 8

Austin Group Defect 1125 is applied, changing "Local timezone information is used" to "Local timezone information shall be set".

Austin Group Defect 1302 is applied, aligning the `localtime()` function with the ISO/IEC 9899:2018 standard.

Austin Group Defect 1376 is applied, removing CX shading from some text derived from the ISO C standard and updating it to match the ISO C standard.

Austin Group Defect 1533 is applied, adding `tm_gmtoff` and `tm_zone` to the `tm` structure.

Austin Group Defect 1570 is applied, removing extra spacing in "==".

---

*The Open Group Base Specifications Issue 8*
*IEEE Std 1003.1-2024*
*Copyright © 2001-2024 The IEEE and The Open Group*