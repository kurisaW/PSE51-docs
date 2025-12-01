# mktime — convert broken-down time into time since the Epoch

## SYNOPSIS

```c
#include <time.h>

time_t mktime(struct tm *timeptr);
```

## DESCRIPTION

The `mktime()` function shall convert the broken-down time, expressed as local time, in some members of the structure pointed to by `timeptr`, into a time since the Epoch value with the same encoding as that of the values returned by `time()`. The `mktime()` function shall make use of only the `tm_year`, `tm_mon`, `tm_mday`, `tm_hour`, `tm_min`, `tm_sec`, and `tm_isdst` members of the structure pointed to by `timeptr`; the values of these members shall not be restricted to the ranges described in `<time.h>`.

Local timezone information shall be set as though `mktime()` called `tzset()`.

The `mktime()` function shall calculate the time in seconds since the Epoch to be returned as if by manipulating the members of the **tm** structure according to the following steps:

1. The `tm_sec` member may, but should not, be brought into the range 0 to 60, inclusive. For each 60 seconds added to or subtracted from `tm_sec`, a decrement or increment, respectively, of 1 minute shall be saved for later application.

2. The `tm_min` member shall be brought into the range 0 to 59, inclusive, and any saved decrement or increment of minutes shall then be applied, repeating the range adjustment afterwards if necessary. For each 60 minutes added to or subtracted from `tm_min`, a decrement or increment, respectively, of 1 hour shall be saved for later application.

3. The `tm_hour` member shall be brought into the range 0 to 23, inclusive, and any saved decrement or increment of hours shall then be applied, repeating the range adjustment afterwards if necessary. For each 24 hours added to or subtracted from `tm_hour`, a decrement or increment, respectively, of 1 day shall be saved for later application.

4. The `tm_mon` member shall be brought into the range 0 to 11, inclusive. For each 12 months added to or subtracted from `tm_mon`, a decrement or increment, respectively, of 1 year shall be saved for later use.

5. The `tm_mday` member shall be brought into the range 1 to 31, inclusive, and any saved decrement or increment of days shall then be applied, repeating the range adjustment afterwards if necessary. Adjustments downwards shall be applied by subtracting the number of days (according to the Gregorian calendar) in month `tm_mon`+1 of the year obtained by adding/subtracting any saved increment/decrement of years to the value `tm_year`+1900, and then incrementing `tm_mon` by 1, repeated as necessary. Adjustments upwards shall be applied by adding the number of days in the month before month `tm_mon`+1 of the year obtained by adding/subtracting any saved increment/decrement of years to the value `tm_year`+1900, and then decrementing `tm_mon` by 1, repeated as necessary. During these adjustments, the `tm_mon` value shall be kept within the range 0 to 11, inclusive, by applying step 4 as necessary.

6. If the `tm_mday` member is greater than the number of days in month `tm_mon`+1 of the year obtained by adding/subtracting any saved increment/decrement of years to the value `tm_year`+1900, that number of days shall be subtracted from `tm_mday`, and `tm_mon` shall be incremented by 1. If this results in `tm_mon` having the value 12, step 4 shall be applied.

7. The number of seconds since the Epoch in Coordinated Universal Time shall be calculated from the range-corrected values of the relevant **tm** structure members (or the original value where a member was not range corrected) as specified in the expression given in the definition of seconds since the Epoch (see XBD 4.19 Seconds Since the Epoch), where the names other than `tm_year` and `tm_yday` in the structure and in the expression correspond, the `tm_year` value used in the expression is the `tm_year` in the structure plus/minus any saved increment/decrement of years, and the `tm_yday` value used in the expression is the day of the year from 0 to 365 inclusive, calculated from the `tm_mon` and `tm_mday` members of the **tm** structure, for that year.

8. The time since the Epoch shall be corrected for the offset of the local timezone's standard time from Coordinated Universal Time.

9. The time since the Epoch shall be further corrected (if applicable—see below) for Daylight Saving Time.

If the timezone is one that includes Daylight Saving Time (DST) adjustments, the value of `tm_isdst` in the **tm** structure controls whether or not `mktime()` adjusts the calculated seconds since the Epoch value by the DST offset (after it has made the timezone adjustment), as follows:

* If `tm_isdst` is zero, `mktime()` shall not further adjust the seconds since the Epoch by the DST offset.
* If `tm_isdst` is positive, `mktime()` shall further adjust the seconds since the Epoch by the DST offset.
* If `tm_isdst` is negative, `mktime()` shall attempt to determine whether DST is in effect for the specified time; if it determines that DST is in effect it shall produce the same result as an equivalent call with a positive `tm_isdst` value, otherwise it shall produce the same result as an equivalent call with a `tm_isdst` value of zero. If the broken-down time specifies a time that is either skipped over or repeated when a transition to or from DST occurs, it is unspecified whether `mktime()` produces the same result as an equivalent call with a positive `tm_isdst` value or as an equivalent call with a `tm_isdst` value of zero.

If the `TZ` environment variable specifies a geographical timezone for which the implementation's timezone database includes historical or future changes to the offset from Coordinated Universal Time of the timezone's standard time, and the broken-down time corresponds to a time that was (or will be) skipped over or repeated due to the occurrence of such a change, `mktime()` shall calculate the time since the Epoch value using either the offset in effect before the change or the offset in effect after the change.

Upon successful completion, the members of the structure shall be set to the values that would be returned by a call to `localtime()` with the calculated time since the Epoch as its argument.

## RETURN VALUE

The `mktime()` function shall return the calculated time since the Epoch encoded as a value of type **time_t**. If the time since the Epoch cannot be represented as a **time_t** or the value to be returned in the `tm_year` member of the structure pointed to by `timeptr` cannot be represented as an **int**, the function shall return the value (**time_t**)-1 and set `errno` to [EOVERFLOW], and shall not change the value of the `tm_wday` component of the structure.

Since (**time_t**)-1 is a valid return value for a successful call to `mktime()`, an application wishing to check for error situations should set `tm_wday` to a value less than 0 or greater than 6 before calling `mktime()`. On return, if `tm_wday` has not changed an error has occurred.

## ERRORS

The `mktime()` function shall fail if:

**[EOVERFLOW]**
The result cannot be represented.

## EXAMPLES

What day of the week is July 4, 2001?

```c
#include <stdio.h>
#include <time.h>

struct tm time_str;

char daybuf[20];

int main(void)
{
    time_str.tm_year = 2001 - 1900;
    time_str.tm_mon = 7 - 1;
    time_str.tm_mday = 4;
    time_str.tm_hour = 0;
    time_str.tm_min = 0;
    time_str.tm_sec = 1;
    time_str.tm_isdst = -1;
    time_str.tm_wday = -1;
    if (mktime(&time_str) == (time_t)-1 && time_str.tm_wday == -1)
        (void)puts("-unknown-");
    else {
        (void)strftime(daybuf, sizeof(daybuf), "%A", &time_str);
        (void)puts(daybuf);
    }
    return 0;
}
```

## APPLICATION USAGE

When using `mktime()` to add or subtract a fixed time period (one that always corresponds to a fixed number of seconds) to or from a broken-down time in the local timezone, reliable results for arbitrary `TZ` can only be assured by using `mktime()` to convert the original broken-down time to a time since the Epoch, adding or subtracting the desired number of seconds to that value, and then calling `localtime()` with the result. The alternative of adjusting the broken-down time before calling `mktime()` may produce unexpected results if the original and updated times are on different sides of a geographical timezone change. On implementations that follow the recommendation of not range-correcting `tm_sec` (see step 1 in the DESCRIPTION), reliable results can also be assured by adding or subtracting the desired number of seconds to `tm_sec` (and not modifying any other members of the **tm** structure). In applications needing to be portable to non-POSIX systems where the **time_t** encoding is not a count of seconds, it is recommended that conditional compilation is used such that the adjustment is performed on the `mktime()` return value when possible, and otherwise on the `tm_sec` member. For timezones that are known not to have geographical timezone changes, such as TZ=UTC0, adjustments using just `mktime()` do not have this problem.

The way the `mktime()` function interprets out-of-range **tm** structure fields might not produce the expected result when multiple adjustments are made at the same time. For example, if an application tries to go back one day first and then one year by calling `localtime()`, decrementing `tm_mday` and `tm_year`, and then calling `mktime()` this would not produce the expected result if it was called on 2021-03-01 because `mktime()` would see the supplied year as 2020 (a leap year) and correct Mar 0 to Feb 29, whereas the intended result was Feb 28. Such issues can be avoided by doing multiple adjustments one at a time when the order in which they are done matters.

Examples of how `mktime()` handles some adjustments are:

* If given Feb 29 in a non-leap year it treats that as the day after Feb 28 and gives back Mar 1.
* If given Feb 0 it treats that as the day before Feb 1 and gives back Jan 31.
* If given 21:65 it treats that as 6 minutes after 21:59 and gives back 22:05.
* If given `tm_isdst`=0 for a time when DST is in effect, it gives back a positive `tm_isdst` and alters the other fields appropriately.
* If there is a DST transition where 02:00 standard time becomes 03:00 DST and `mktime()` is given 02:30 (with negative `tm_isdst`), it treats that as either 30 minutes after 02:00 standard time or 30 minutes before 03:00 DST and gives back a zero or positive `tm_isdst`, respectively, with the `tm_hour` field altered appropriately.
* If a geographical timezone changes its UTC offset such that "old 00:00" becomes "new 00:30" and `mktime()` is given 00:20, it treats that as either 20 minutes after "old 00:00" or 10 minutes before "new 00:30", and gives back appropriately altered **struct tm** fields.

If an application wants to check whether a given broken-down time is one that is skipped over, it can do so by seeing whether the `tm_mday`, `tm_hour`, and `tm_min` values it gets back from `mktime()` are the same ones it fed in. Just checking `tm_hour` and `tm_min` might appear at first sight to suffice, but `tm_mday` could also change—without `tm_hour` and `tm_min` changing—if, for example, `TZ` is set to "ABC12XYZ-12" (which might be used in a torture test) or if a geographical timezone changes the offset from Coordinated Universal Time of its standard time by 24 hours.

## RATIONALE

In order to allow applications to distinguish between a successful return of (**time_t**)-1 and an [EOVERFLOW] error, `mktime()` is required not to change `tm_wday` on error. This mechanism is used rather than the convention used for other functions whereby the application sets `errno` to zero before the call and the call does not change `errno` on error because the ISO C standard does not require `mktime()` to set `errno` on error. The next revision of the ISO C standard is expected to require that `mktime()` does not change `tm_wday` when returning (**time_t**)-1 to indicate an error, and that this return convention is used both for the case where the value to be returned by the function cannot be represented as a **time_t** and the case where the value to be returned in the `tm_year` member of the **tm** structure cannot be represented as an **int**.

The DESCRIPTION section says that `mktime()` converts the specified broken-down time into **a** time since the Epoch value. The use of the indefinite article here is necessary because, when `tm_isdst` is negative and the timezone has Daylight Saving Time transitions, there is not a one-to-one correspondence between broken-down times and time since the Epoch values.

The description of how the value of `tm_isdst` affects the behavior of `mktime()` is shaded CX because the requirements in the ISO C standard are unclear. The next revision of the ISO C standard is expected to state the requirements using wording equivalent to the wording in this standard.

Implementations are encouraged not to range-correct `tm_sec` (see step 1 in the DESCRIPTION) in order for the results of making an adjustment to `tm_sec` always to be equivalent to making the same adjustment to the value returned by `mktime()`, even when the original and updated times are on different sides of a geographical timezone change. This provides a way for applications to do reliable fixed-period adjustment using only `mktime()`, as described in APPLICATION USAGE.

The described method for range-correcting the **tm** structure members uses separate variables to hold adjustment values to be applied later to other members, or (for the year adjustment) used in later calculations, because this is one way of avoiding intermediate member values that are not representable as an **int**. Implementations may use other methods; all that is required is that `tm_year` is the only member for which an [EOVERFLOW] error can occur.

The described method for range-correcting `tm_mday` would, if implemented that way, be highly inefficient for very large values. The efficiency can be improved by observing that any period of 400 years always has the same number of days, so the month-by-month correction method need only be applied for a maximum of 4800 months.

## FUTURE DIRECTIONS

A future version of this standard may require that `mktime()` does not perform the optional range correction of the `tm_sec` member of the **tm** structure described at step 1 in the DESCRIPTION.

A future version of this standard is expected to add a `timegm()` function that is similar to `mktime()`, except that the **tm** structure pointed to by `timeptr` contains a broken-down time in Coordinated Universal Time (rather than the local timezone), where references to `localtime()` are replaced by references to `gmtime()`, and where there are no timezone offset or Daylight Saving Time adjustments. A combination of `gmtime()` and `timegm()` will be the expected way to perform arithmetic upon a **time_t** value and remain compatible with the ISO C standard (where the internal structure of a **time_t** is not specified), since attempting such manipulations using `localtime()` and `mktime()` can lead to unexpected results.

## SEE ALSO

`asctime()`, `clock()`, `ctime()`, `difftime()`, `futimens()`, `gmtime()`, `localtime()`, `strftime()`, `strptime()`, `time()`, `tzset()`

XBD 4.19 Seconds Since the Epoch, `<time.h>`

## CHANGE HISTORY

First released in Issue 3. Included for alignment with the POSIX.1-1988 standard and the ANSI C standard.

### Issue 6

Extensions beyond the ISO C standard are marked.

IEEE Std 1003.1-2001/Cor 2-2004, item XSH/TC2/D6/58 is applied, updating the RETURN VALUE and ERRORS sections to add the optional [EOVERFLOW] error as a CX extension.

IEEE Std 1003.1-2001/Cor 2-2004, item XSH/TC2/D6/59 is applied, adding the `tzset()` function to the SEE ALSO section.

### Issue 7

POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0393 [104] is applied.

POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0228 [724] is applied.

### Issue 8

Austin Group Defect 1253 is applied, changing "Daylight Savings" to "Daylight Saving".

Austin Group Defect 1613 is applied, changing the way the **tm** structure members used by `mktime()` are specified and clarifying that a successful call sets the members to the same values that would be returned by `localtime()`.

Austin Group Defect 1614 is applied, clarifying how `tm_isdst` is handled and the conditions under which (**time_t**)-1 is returned.

Austin Group Defect 1627 is applied, clarifying how `mktime()` calculates the time in seconds since the Epoch from the members of the **tm** structure.