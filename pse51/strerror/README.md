# strerror, strerror_l, strerror_r — get error message string

## SYNOPSIS

```c
#include <string.h>

char *strerror(int errnum);

/* XSI extension */
char *strerror_l(int errnum, locale_t locale);
int strerror_r(int errnum, char *strerrbuf, size_t buflen);
```

## DESCRIPTION

For strerror():
- The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

The strerror() function shall map the error number in errnum to a locale-dependent error message string and shall return a pointer to it. Typically, the values for errnum come from errno, but strerror() shall map any value of type int to a message.

The application shall not modify the string returned. The returned string pointer might be invalidated or the string content might be overwritten by a subsequent call to strerror(), or by a subsequent call to strerror_l() in the same thread. The returned pointer and the string content might also be invalidated if the calling thread is terminated.

The string may be overwritten by a subsequent call to strerror_l() in the same thread.

The contents of the error message strings returned by strerror() should be determined by the setting of the LC_MESSAGES category in the current locale.

The implementation shall behave as if no function defined in this volume of POSIX.1-2024 calls strerror().

The strerror() and strerror_l() functions shall not change the setting of errno if successful.

Since no return value is reserved to indicate an error of strerror(), an application wishing to check for error situations should set errno to 0, then call strerror(), then check errno. Similarly, since strerror_l() is required to return a string for some errors, an application wishing to check for all error situations should set errno to 0, then call strerror_l(), then check errno.

The strerror() function need not be thread-safe; however, strerror() shall avoid data races with all other functions.

The strerror_l() function shall map the error number in errnum to a locale-dependent error message string in the locale represented by locale and shall return a pointer to it.

The strerror_r() function shall map the error number in errnum to a locale-dependent error message string and shall return the string in the buffer pointed to by strerrbuf, with length buflen.

If the value of errnum is a valid error number, the message string shall indicate what error occurred; if the value of errnum is zero, the message string shall either be an empty string or indicate that no error occurred; otherwise, if these functions complete successfully, the message string shall indicate that an unknown error occurred.

The behavior is undefined if the locale argument to strerror_l() is the special locale object LC_GLOBAL_LOCALE or is not a valid locale object handle.

## RETURN VALUE

Upon completion, whether successful or not, strerror() shall return a pointer to the generated message string. On error errno may be set, but no return value is reserved to indicate an error.

Upon successful completion, strerror_l() shall return a pointer to the generated message string. If errnum is not a valid error number, errno may be set to [EINVAL], but a pointer to a message string shall still be returned. If any other error occurs, errno shall be set to indicate the error and a null pointer shall be returned.

Upon successful completion, strerror_r() shall return 0. Otherwise, an error number shall be returned to indicate the error.

## ERRORS

These functions may fail if:

- **EINVAL** - The value of errnum is neither a valid error number nor zero.

The strerror_r() function shall fail if:

- **ERANGE** - Insufficient storage was supplied via strerrbuf and buflen to contain the generated message string.

## EXAMPLES

None.

## APPLICATION USAGE

Historically in some implementations, calls to perror() would overwrite the string that the pointer returned by strerror() points to. Such implementations did not conform to the ISO C standard; however, application developers should be aware of this behavior if they wish their applications to be portable to such implementations.

Applications should use strerror_l() rather than strerror() or strerror_r() to avoid thread safety and possible alternative (non-conforming) versions of these functions in some implementations.

## RATIONALE

The strerror_l() function is required to be thread-safe, thereby eliminating the need for an equivalent to the strerror_r() function.

Earlier versions of this standard did not explicitly require that the error message strings returned by strerror() and strerror_r() provide any information about the error. This version of the standard requires a meaningful message for any successful completion.

Since no return value is reserved to indicate a strerror() error, but all calls (whether successful or not) must return a pointer to a message string, on error strerror() can return a pointer to an empty string or a pointer to a meaningful string that can be printed.

Note that the [EINVAL] error condition is a may fail error. If an invalid error number is supplied as the value of errnum, applications should be prepared to handle any of the following:

1. **Error (with no meaningful message)**: errno is set to [EINVAL], the return value is a pointer to an empty string.

2. **Successful completion**: errno is unchanged and the return value points to a string like "unknown error" or "error number xxx" (where xxx is the value of errnum).

3. **Combination of #1 and #2**: errno is set to [EINVAL] and the return value points to a string like "unknown error" or "error number xxx" (where xxx is the value of errnum). Since applications frequently use the return value of strerror() as an argument to functions like fprintf() (without checking the return value) and since applications have no way to parse an error message string to determine whether errnum represents a valid error number, implementations are encouraged to implement #3. Similarly, implementations are encouraged to have strerror_r() return [EINVAL] and put a string like "unknown error" or "error number xxx" in the buffer pointed to by strerrbuf when the value of errnum is not a valid error number.

Additionally, implementations are encouraged to null terminate strerrbuf when failing with [ERANGE] for any size other than buflen of zero.

Some applications rely on being able to set errno to 0 before calling a function with no reserved value to indicate an error, then call strerror(errno) afterwards to detect whether an error occurred (because errno changed) or to indicate success (because errno remained zero). This usage pattern requires that strerror(0) succeed with useful results. Previous versions of the standard did not specify the behavior when errnum is zero.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- [perror()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/perror.html)
- [<string.h>](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/string.h.html)

## CHANGE HISTORY

First released in Issue 3.

### Issue 5

The DESCRIPTION is updated to indicate that errno is not changed if the function is successful.

A note indicating that the strerror() function need not be reentrant is added to the DESCRIPTION.

### Issue 6

Extensions beyond the ISO C standard are marked.

The following new requirements on POSIX implementations derive from alignment with the Single UNIX Specification:

- In the RETURN VALUE section, the fact that errno may be set is added.
- The [EINVAL] optional error condition is added.

The normative text is updated to avoid use of the term "must" for application requirements.

The strerror_r() function is added in response to IEEE PASC Interpretation 1003.1c #39.

The strerror_r() function is marked as part of the Thread-Safe Functions option.

### Issue 7

Austin Group Interpretation 1003.1-2001 #072 is applied, updating the ERRORS section.

Austin Group Interpretation 1003.1-2001 #156 is applied.

Austin Group Interpretation 1003.1-2001 #187 is applied, clarifying the behavior when the generated error message is an empty string.

SD5-XSH-ERN-191 is applied, updating the APPLICATION USAGE section.

The strerror_l() function is added from The Open Group Technical Standard, 2006, Extended API Set Part 4.

The strerror_r() function is moved from the Thread-Safe Functions option to the Base.

POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0595 [75], XSH/TC1-2008/0596 [447], XSH/TC1-2008/0597 [382,428], XSH/TC1-2008/0598 [283], XSH/TC1-2008/0599 [382,428], XSH/TC1-2008/0600 [283], and XSH/TC1-2008/0601 [382,428] are applied.

POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0339 [656] is applied.

### Issue 8

Austin Group Defect 398 is applied, changing the [ERANGE] error from "may fail" to "shall fail".

Austin Group Defect 655 is applied, changing the APPLICATION USAGE section.

Austin Group Defect 1302 is applied, aligning the strerror() function with the ISO/IEC 9899:2018 standard.

---

UNIX® is a registered Trademark of The Open Group.
POSIX™ is a Trademark of The IEEE.
Copyright © 2001-2024 The IEEE and The Open Group, All Rights Reserved