# strftime, strftime_l — convert date and time to a string

## SYNOPSIS

```c
#include <time.h>

size_t strftime(char *restrict s, size_t maxsize,
               const char *restrict format, const struct tm *restrict timeptr);

[CX] size_t strftime_l(char *restrict s, size_t maxsize,
                      const char *restrict format, const struct tm *restrict timeptr,
                      locale_t locale);
```

## DESCRIPTION

For `strftime()`: [CX] The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

The `strftime()` function shall place bytes into the array pointed to by `s` as controlled by the string pointed to by `format`. The application shall ensure that the format is a character string, beginning and ending in its initial shift state, if any. The format string consists of zero or more conversion specifications and ordinary characters.

Each conversion specification is introduced by the '%' character after which the following appear in sequence:

*   [CX] An optional flag:
    *   `0` - The zero character ('0'), which specifies that the character used as the padding character is '0'
    *   `+` - The <plus-sign> character ('+'), which specifies that the character used as the padding character is '0', and that if and only if the field being produced consumes more than four bytes to represent a year (for %F, %G, or %Y) or more than two bytes to represent the year divided by 100 (for %C) then a leading <plus-sign> character shall be included if the year being processed is greater than or equal to zero or a leading <hyphen-minus> character ('-') shall be included if the year is less than zero.
    *   The default padding character is unspecified.

*   An optional minimum field width. If the converted value, including any leading '+' or '-' sign, has fewer bytes than the minimum field width and the padding character is not the NUL character, the output shall be padded on the left (after any leading '+' or '-' sign) with the padding character.

*   An optional E or O modifier.

*   A terminating conversion specifier character that indicates the type of conversion to be applied.

[CX] The results are unspecified if more than one flag character is specified, a flag character is specified without a minimum field width; a minimum field width is specified without a flag character; a modifier is specified with a flag or with a minimum field width; or if a minimum field width is specified for any conversion specifier other than C, F, G, or Y.

All ordinary characters (including the terminating NUL character) are copied unchanged into the array. If copying takes place between objects that overlap, the behavior is undefined. No more than `maxsize` bytes are placed into the array. Each conversion specifier is replaced by appropriate characters as described in the following list. The appropriate characters are determined using the `LC_TIME` category of the current locale and by the values of zero or more members of the broken-down time structure pointed to by `timeptr`, as specified in brackets in the description. If any of the specified values are outside the normal range, the characters stored are unspecified.

[CX] The `strftime_l()` function shall be equivalent to the `strftime()` function, except that the locale data used is from the locale represented by `locale`.

Local timezone information shall be set as though `strftime()` called `tzset()`.

### Conversion Specifiers

The following conversion specifiers shall be supported:

**%a** - Replaced by the locale's abbreviated weekday name. [`tm_wday`]

**%A** - Replaced by the locale's full weekday name. [`tm_wday`]

**%b** - Replaced by the locale's abbreviated month name. [`tm_mon`]

**%B** - Replaced by the locale's full month name. [`tm_mon`]

**%c** - Replaced by the locale's appropriate date and time representation.

**%C** - Replaced by the year divided by 100 and truncated to an integer, as a decimal number. [`tm_year`]
-   If a minimum field width is not specified:
    *   If the year is between 0 and 9999 inclusive, two characters shall be placed into the array pointed to by `s`, including a leading '0' if there would otherwise be only a single digit.
    *   [CX] If the year is less than 0 or greater than 9999, the number of characters placed into the array pointed to by `s` shall be the number of digits and leading sign characters (if any) in the result of dividing the year by 100 and truncating, or two, whichever is greater.

**%d** - Replaced by the day of the month as a decimal number [01,31]. [`tm_mday`]

**%D** - Equivalent to %m/%d/%y. [`tm_mon`, `tm_mday`, `tm_year`]

**%e** - Replaced by the day of the month as a decimal number [1,31]; a single digit is preceded by a space. [`tm_mday`]

**%F** - Equivalent to %Y-%m-%d if no flag and no minimum field width are specified. (For years between 1000 and 9999 inclusive this provides the ISO 8601:2019 standard complete representation, extended format date representation of a specific day.) [`tm_year`, `tm_mon`, `tm_mday`]

**%g** - Replaced by the last 2 digits of the week-based year as a decimal number [00,99]. [`tm_year`, `tm_wday`, `tm_yday`]

**%G** - Replaced by the week-based year as a decimal number (for example, 1977). [`tm_year`, `tm_wday`, `tm_yday`]

**%h** - Equivalent to %b. [`tm_mon`]

**%H** - Replaced by the hour (24-hour clock) as a decimal number [00,23]. [`tm_hour`]

**%I** - Replaced by the hour (12-hour clock) as a decimal number [01,12]. [`tm_hour`]

**%j** - Replaced by the day of the year as a decimal number [001,366]. [`tm_yday`]

**%m** - Replaced by the month as a decimal number [01,12]. [`tm_mon`]

**%M** - Replaced by the minute as a decimal number [00,59]. [`tm_min`]

**%n** - Replaced by a <newline>.

**%p** - Replaced by the locale's equivalent of either a.m. or p.m. [`tm_hour`]

**%r** - Replaced by the time in 12-hour clock notation; [CX] if the 12-hour format is not supported in the locale, this shall be either an empty string or the time in a 24-hour clock notation. In the POSIX locale this shall be equivalent to %I:%M:%S %p. [`tm_hour`, `tm_min`, `tm_sec`]

**%R** - Replaced by the time in 24-hour notation (%H:%M). [`tm_hour`, `tm_min`]

**%s** - [CX] Replaced by the number of seconds since the Epoch as a decimal number, calculated as described for `mktime()`. [`tm_year`, `tm_mon`, `tm_mday`, `tm_hour`, `tm_min`, `tm_sec`, `tm_isdst`]

**%S** - Replaced by the second as a decimal number [00,60]. [`tm_sec`]

**%t** - Replaced by a <tab>.

**%T** - Replaced by the time (%H:%M:%S). [`tm_hour`, `tm_min`, `tm_sec`]

**%u** - Replaced by the weekday as a decimal number [1,7], with 1 representing Monday. [`tm_wday`]

**%U** - Replaced by the week number of the year as a decimal number [00,53]. The first Sunday of January is the first day of week 1; days in the new year before this are in week 0. [`tm_year`, `tm_wday`, `tm_yday`]

**%V** - Replaced by the week number of the year (Monday as the first day of the week) as a decimal number [01,53]. If the week containing 1 January has four or more days in the new year, then it is considered week 1. Otherwise, it is the last week of the previous year, and the next week is week 1. Both January 4th and the first Thursday of January are always in week 1. [`tm_year`, `tm_wday`, `tm_yday`]

**%w** - Replaced by the weekday as a decimal number [0,6], with 0 representing Sunday. [`tm_wday`]

**%W** - Replaced by the week number of the year as a decimal number [00,53]. The first Monday of January is the first day of week 1; days in the new year before this are in week 0. [`tm_year`, `tm_wday`, `tm_yday`]

**%x** - Replaced by the locale's appropriate date representation.

**%X** - Replaced by the locale's appropriate time representation.

**%y** - Replaced by the last two digits of the year as a decimal number [00,99]. [`tm_year`]

**%Y** - Replaced by the year as a decimal number (for example, 1997). [`tm_year`]

**%z** - Replaced by the offset from UTC in the ISO 8601:2019 standard format (+hhmm or -hhmm), or by no characters if no timezone is determinable. For example, "-0430" means 4 hours 30 minutes behind UTC (west of Greenwich). [CX] If `tm_isdst` is zero, the standard time offset is used. If `tm_isdst` is greater than zero, the daylight saving time offset is used. If `tm_isdst` is negative, no characters are returned. [`tm_isdst`, [CX] `tm_gmtoff`]

**%Z** - Replaced by the timezone name or abbreviation, or by no bytes if no timezone information exists. [`tm_isdst`, [CX] `tm_zone`]

**%%** - Replaced by %.

If a conversion specification does not correspond to any of the above, the behavior is undefined.

[CX] If a `struct tm` broken-down time structure is created by `localtime()` or `localtime_r()`, or modified by `mktime()`, and the value of `TZ` is subsequently modified, the results of the %Z and %z `strftime()` conversion specifiers are undefined, when `strftime()` is called with such a broken-down time structure.

If a `struct tm` broken-down time structure is created or modified by `gmtime()` or `gmtime_r()`, it is unspecified whether the result of the %Z and %z conversion specifiers shall refer to UTC or the current local timezone, when `strftime()` is called with such a broken-down time structure.

### Modified Conversion Specifiers

Some conversion specifiers can be modified by the E or O modifier characters to indicate that an alternative format or specification should be used rather than the one normally used by the unmodified conversion specifier. If the alternative format or specification does not exist for the current locale, the behavior shall be as if the unmodified conversion specification were used.

**%Ec** - Replaced by the locale's alternative appropriate date and time representation.

**%EC** - Replaced by the name of the base year (period) in the locale's alternative representation.

**%Ex** - Replaced by the locale's alternative date representation.

**%EX** - Replaced by the locale's alternative time representation.

**%Ey** - Replaced by the offset from %EC (year only) in the locale's alternative representation.

**%EY** - Replaced by the full alternative year representation.

**%Ob** - [CX] Replaced by the locale's abbreviated alternative month name.

**%OB** - [CX] Replaced by the locale's alternative appropriate full month name.

**%Od** - Replaced by the day of the month, using the locale's alternative numeric symbols, filled as needed with leading zeros if there is any alternative symbol for zero; otherwise, with leading <space> characters.

**%Oe** - Replaced by the day of the month, using the locale's alternative numeric symbols, filled as needed with leading <space> characters.

**%OH** - Replaced by the hour (24-hour clock) using the locale's alternative numeric symbols.

**%OI** - Replaced by the hour (12-hour clock) using the locale's alternative numeric symbols.

**%Om** - Replaced by the month using the locale's alternative numeric symbols.

**%OM** - Replaced by the minutes using the locale's alternative numeric symbols.

**%OS** - Replaced by the seconds using the locale's alternative numeric symbols.

**%Ou** - Replaced by the weekday as a number in the locale's alternative representation (Monday=1).

**%OU** - Replaced by the week number of the year (Sunday as the first day of the week, rules corresponding to %U) using the locale's alternative numeric symbols.

**%OV** - Replaced by the week number of the year (Monday as the first day of the week, rules corresponding to %V) using the locale's alternative numeric symbols.

**%Ow** - Replaced by the number of the weekday (Sunday=0) using the locale's alternative numeric symbols.

**%OW** - Replaced by the week number of the year (Monday as the first day of the week) using the locale's alternative numeric symbols.

**%Oy** - Replaced by the year (offset from %C) using the locale's alternative numeric symbols.

%g, %G, and %V give values according to the ISO 8601:2019 standard week-based year. In this system, weeks begin on a Monday and week 1 of the year is the week that includes January 4th, which is also the week that includes the first Thursday of the year, and is also the first week that contains at least four days in the year. If the first Monday of January is the 2nd, 3rd, or 4th, the preceding days are part of the last week of the preceding year; thus, for Saturday 2nd January 1999, %G is replaced by 1998 and %V is replaced by 53. If December 29th, 30th, or 31st is a Monday, it and any following days are part of week 1 of the following year. Thus, for Tuesday 30th December 1997, %G is replaced by 1998 and %V is replaced by 01.

[CX] The behavior is undefined if the `locale` argument to `strftime_l()` is the special locale object LC_GLOBAL_LOCALE or is not a valid locale object handle.

## RETURN VALUE

If successful, these functions shall return the number of bytes placed into the array pointed to by `s`, not including the terminating NUL character. [CX] If successful, `errno` shall not be changed. Otherwise, 0 shall be returned, [CX] `errno` shall be set to indicate the error, and the contents of the array are unspecified.

## ERRORS

[CX] These functions shall fail if:

- **[ERANGE]** - The total number of resulting bytes including the terminating NUL character is more than `maxsize`.

These functions may fail if:

- **[EINVAL]** - The `format` string includes a %s conversion and the number of seconds since the Epoch would be negative.
- **[EOVERFLOW]** - The `format` string includes a %s conversion and the number of seconds since the Epoch cannot be represented in a `time_t`.

## EXAMPLES

### Getting a Localized Date String

The following example first sets the locale to the user's default. The locale information will be used in the `nl_langinfo()` and `strftime()` functions. The `nl_langinfo()` function returns the localized date string which specifies how the date is laid out. The `strftime()` function takes this information and, using the `tm` structure for values, places the date and time information into `datestring`.

```c
#include <time.h>
#include <locale.h>
#include <langinfo.h>
...
struct tm *tm;
char datestring[256];
...
setlocale (LC_ALL, "");
...
strftime (datestring, sizeof(datestring), nl_langinfo (D_T_FMT), tm);
```

## APPLICATION USAGE

A return value of 0 may indicate either success or failure if a format is the empty string or consists of conversion specifications such as %p or %Z that are replaced by no characters in the current locale or because of the current setting of `tzname[]`, respectively. To distinguish between success and failure when `strftime()` returns 0, an application can set `errno` to 0 before calling `strftime()` and test whether `errno` is 0 afterwards.

The range of values for %S is [00,60] rather than [00,59] to allow for the occasional leap second.

Some of the conversion specifications are duplicates of others. They are included for compatibility with `nl_cxtime()` and `nl_ascxtime()`, which were published in Issue 2.

The %C, %F, %G, and %Y format specifiers in `strftime()` always print full values, but the `strptime()` %C, %F, and %Y format specifiers only scan two digits (assumed to be the first two digits of a four-digit year) for %C and four digits (assumed to be the entire (four-digit) year) for %F and %Y. This mimics the behavior of `printf()` and `scanf()`.

In the C or POSIX locale, the E and O modifiers are ignored and the replacement strings for the following specifiers are:

- **%a** - The first three characters of %A.
- **%A** - One of Sunday, Monday, ..., Saturday.
- **%b** - The first three characters of %B.
- **%B** - One of January, February, ..., December.
- **%c** - Equivalent to %a %b %e %T %Y.
- **%p** - One of AM or PM.
- **%r** - Equivalent to %I:%M:%S %p.
- **%x** - Equivalent to %m/%d/%y.
- **%X** - Equivalent to %T.
- **%Z** - Implementation-defined.

## SEE ALSO

`asctime()`, `clock()`, `ctime()`, `difftime()`, `futimens()`, `getdate()`, `gmtime()`, `localtime()`, `mktime()`, `strptime()`, `time()`, `tzset()`, `uselocale()`

XBD 7.3.5 LC_TIME, `<time.h>`