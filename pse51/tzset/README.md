# tzset - set timezone conversion information

## SYNOPSIS

```c
#include <time.h>

[XSI] extern int daylight;
[XSI] extern long timezone;

[CX] extern char *tzname[2];
[CX] void tzset(void);
```

## DESCRIPTION

The `tzset()` function shall use the value of the environment variable `TZ` to set time conversion information used by `ctime()`, `localtime()`, `mktime()`, and `strftime()`. If `TZ` is absent from the environment, implementation-defined default timezone information shall be used.

The `tzset()` function shall set the external variable `tzname` as follows:

```c
tzname[0] = "std";
tzname[1] = "dst";
```

where `std` and `dst` are as described in XBD 8. Environment Variables.

[XSI] The `tzset()` function also shall set the external variable `daylight` to 0 if Daylight Saving Time conversions should never be applied for the timezone in use; otherwise, non-zero. The external variable `timezone` shall be set to the difference, in seconds, between Coordinated Universal Time (UTC) and local standard time.

If a thread accesses `tzname`, [XSI] `daylight`, or `timezone` directly while another thread is in a call to `tzset()`, or to any function that is required or allowed to set timezone information as if by calling `tzset()`, the behavior is undefined.

## RETURN VALUE

The `tzset()` function shall not return a value.

## ERRORS

No errors are defined.

## EXAMPLES

Example `TZ` variables and their timezone differences are given in the table below:

| TZ | timezone |
|----|----------|
| EST5EDT | 5\*60\*60 |
| GMT0 | 0\*60\*60 |
| JST-9 | -9\*60\*60 |
| MET-1MEST | -1\*60\*60 |
| MST7MDT | 7\*60\*60 |
| PST8PDT | 8\*60\*60 |

## APPLICATION USAGE

Since the `ctime()`, `localtime()`, `mktime()`, `strftime()`, and `strftime_l()` functions are required to set timezone information as if by calling `tzset()`, there is no need for an explicit `tzset()` call before using these functions. However, portable applications should call `tzset()` explicitly before using `localtime_r()` because setting timezone information is optional for that function.

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- `ctime()`
- `localtime()`
- `mktime()`
- `strftime()`
- XBD 8. Environment Variables
- `<time.h>`

## CHANGE HISTORY

First released in Issue 1. Derived from Issue 1 of the SVID.

### Issue 6

The example is corrected.

### Issue 7

POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0377 [880] is applied.

### Issue 8

Austin Group Defect 1253 is applied, changing "Daylight Savings" to "Daylight Saving".

Austin Group Defect 1410 is applied, removing the `ctime_r()` function.