# sprintf - print formatted output

## SYNOPSIS

```c
#include <stdio.h>

int asprintf(char **restrict ptr, const char *restrict format, ...);
int dprintf(int fildes, const char *restrict format, ...);

int fprintf(FILE *restrict stream, const char *restrict format, ...);
int printf(const char *restrict format, ...);
int snprintf(char *restrict s, size_t n, const char *restrict format, ...);
int sprintf(char *restrict s, const char *restrict format, ...);
```

## DESCRIPTION

The `fprintf()` function shall place output on the named output `stream`. The `printf()` function shall place output on the standard output stream `stdout`. The `sprintf()` function shall place output followed by the null byte, '\0', in consecutive bytes starting at *s; it is the user's responsibility to ensure that enough space is available.

The `asprintf()` function shall be equivalent to `sprintf()`, except that the output string shall be written to dynamically allocated memory, allocated as if by a call to `malloc()`, of sufficient length to hold the resulting string, including a terminating null byte. If the call to `asprintf()` is successful, the address of this dynamically allocated string shall be stored in the location referenced by `ptr`.

The `dprintf()` function shall be equivalent to the `fprintf()` function, except that `dprintf()` shall write output to the file associated with the file descriptor specified by the `fildes` argument rather than place output on a stream.

The `snprintf()` function shall be equivalent to `sprintf()`, with the addition of the `n` argument which limits the number of bytes written to the buffer referred to by `s`. If `n` is zero, nothing shall be written and `s` may be a null pointer. Otherwise, output bytes beyond the `n`-1st shall be discarded instead of being written to the array, and a null byte is written at the end of the bytes actually written into the array.

If copying takes place between objects that overlap as a result of a call to `sprintf()` or `snprintf()`, the results are undefined.

Each of these functions converts, formats, and prints its arguments under control of the `format`. The application shall ensure that the format is a character string, beginning and ending in its initial shift state, if any. The format is composed of zero or more directives: *ordinary characters*, which are simply copied to the output stream, and *conversion specifications*, each of which shall result in the fetching of zero or more arguments. The results are undefined if there are insufficient arguments for the format. If the format is exhausted while arguments remain, the excess arguments shall be evaluated but are otherwise ignored.

Conversions can be applied to the nth argument after the format in the argument list, rather than to the next unused argument. In this case, the conversion specifier character % (see below) is replaced by the sequence "%n$", where n is a decimal integer in the range [1,{NL_ARGMAX}], giving the position of the argument in the argument list. This feature provides for the definition of format strings that select arguments in an order appropriate to specific languages (see the EXAMPLES section).

The format can contain either numbered argument conversion specifications (that is, those introduced by "%n$" and optionally containing the "*m$" forms of field width and precision), or unnumbered argument conversion specifications (that is, those introduced by the % character and optionally containing the * form of field width and precision), but not both. The only exception to this is that %% can be mixed with the "%n$" form. The results of mixing numbered and unnumbered argument specifications in a format string are undefined. When numbered argument specifications are used, specifying the Nth argument requires that all the leading arguments, from the first to the (N-1)th, are specified in the format string.

In format strings containing the "%n$" form of conversion specification, numbered arguments in the argument list can be referenced from the format string as many times as required.

In format strings containing the % form of conversion specification, each conversion specification uses the first unused argument in the argument list.

All forms of the `fprintf()` functions allow for the insertion of a language-dependent radix character in the output string. The radix character is defined in the current locale (category LC_NUMERIC). In the POSIX locale, or in a locale where the radix character is not defined, the radix character shall default to a period ('.').

Each conversion specification is introduced by the '%' character or by the character sequence "%n$", after which the following appear in sequence:

* Zero or more flags (in any order), which modify the meaning of the conversion specification.
* An optional minimum field width. If the converted value has fewer bytes than the field width, it shall be padded with space characters by default on the left; it shall be padded on the right if the left-adjustment flag ('-'), described below, is given to the field width. The field width takes the form of an asterisk ('*'), or in conversion specifications introduced by "%n$" the "*m$" string, described below, or a decimal integer.
* An optional precision that gives the minimum number of digits to appear for the d, i, o, u, x, and X conversion specifiers; the number of digits to appear after the radix character for the a, A, e, E, f, and F conversion specifiers; the maximum number of significant digits for the g and G conversion specifiers; or the maximum number of bytes to be printed from a string in the s conversion specifiers. The precision takes the form of a period ('.') followed either by an asterisk ('*'), or in conversion specifications introduced by "%n$" the "*m$" string, described below, or an optional decimal digit string, where a null digit string is treated as zero. If a precision appears with any other conversion specifier, the behavior is undefined.
* An optional length modifier that specifies the size of the argument.
* A conversion specifier character that indicates the type of conversion to be applied.

A field width, or precision, or both, may be indicated by an asterisk ('*'). In this case an argument of type `int` supplies the field width or precision. Applications shall ensure that arguments specifying field width, or precision, or both appear in that order before the argument, if any, to be converted. A negative field width is taken as a '-' flag followed by a positive field width. A negative precision is taken as if the precision were omitted. In format strings containing conversion specifications introduced by "%n$", in addition to being indicated by the decimal digit string, a field width may be indicated by the sequence "*m$" and precision by the sequence ".*m$", where m is a decimal integer in the range [1,{NL_ARGMAX}] giving the position in the argument list (after the format argument) of an integer argument containing the field width or precision, for example:

```c
printf("%1$d:%2$.*3$d:%4$.*3$d\n", hour, min, precision, sec);
```

### Flag Characters

The flag characters and their meanings are:

**'** (apostrophe)
The integer portion of the result of a decimal conversion (%i, %d, %u, %f, %F, %g, or %G) shall be formatted with thousands' grouping characters. For other conversions the behavior is undefined. The non-monetary grouping character is used.

**-**
The result of the conversion shall be left-justified within the field. The conversion is right-justified if this flag is not specified.

**+**
The result of a signed conversion shall always begin with a sign ('+' or '-'). The conversion shall begin with a sign only when a negative value is converted if this flag is not specified.

**space**
If the first character of a signed conversion is not a sign or if a signed conversion results in no characters, a space shall be prefixed to the result. This means that if the space and '+' flags both appear, the space flag shall be ignored.

**#**
Specifies that the value is to be converted to an alternative form. For o conversion, it shall increase the precision, if and only if necessary, to force the first digit of the result to be a zero (if the value and precision are both 0, a single 0 is printed). For x or X conversion specifiers, a non-zero result shall have 0x (or 0X) prefixed to it. For a, A, e, E, f, F, g, and G conversion specifiers, the result shall always contain a radix character, even if no digits follow the radix character. Without this flag, a radix character appears in the result of these conversions only if a digit follows it. For g and G conversion specifiers, trailing zeros shall not be removed from the result as they normally are. For other conversion specifiers, the behavior is undefined.

**0**
For d, i, o, u, x, X, a, A, e, E, f, F, g, and G conversion specifiers, leading zeros (following any indication of sign or base) are used to pad to the field width rather than performing space padding, except when converting an infinity or NaN. If the '0' and '-' flags both appear, the '0' flag is ignored. For d, i, o, u, x, and X conversion specifiers, if a precision is specified, the '0' flag shall be ignored. If the '0' and apostrophe flags both appear, the grouping characters are inserted before zero padding. For other conversions, the behavior is undefined.

### Length Modifiers

The length modifiers and their meanings are:

**hh**
Specifies that a following d, i, o, u, x, or X conversion specifier applies to a `signed char` or `unsigned char` argument (the argument will have been promoted according to the integer promotions, but its value shall be converted to `signed char` or `unsigned char` before printing); or that a following n conversion specifier applies to a pointer to a `signed char` argument.

**h**
Specifies that a following d, i, o, u, x, or X conversion specifier applies to a `short` or `unsigned short` argument (the argument will have been promoted according to the integer promotions, but its value shall be converted to `short` or `unsigned short` before printing); or that a following n conversion specifier applies to a pointer to a `short` argument.

**l** (ell)
Specifies that a following d, i, o, u, x, or X conversion specifier applies to a `long` or `unsigned long` argument; that a following n conversion specifier applies to a pointer to a `long` argument; that a following c conversion specifier applies to a `wint_t` argument; that a following s conversion specifier applies to a pointer to a `wchar_t` argument; or has no effect on a following a, A, e, E, f, F, g, or G conversion specifier.

**ll** (ell-ell)
Specifies that a following d, i, o, u, x, or X conversion specifier applies to a `long long` or `unsigned long long` argument; or that a following n conversion specifier applies to a pointer to a `long long` argument.

**j**
Specifies that a following d, i, o, u, x, or X conversion specifier applies to an `intmax_t` or `uintmax_t` argument; or that a following n conversion specifier applies to a pointer to an `intmax_t` argument.

**z**
Specifies that a following d, i, o, u, x, or X conversion specifier applies to a `size_t` or the corresponding signed integer type argument; or that a following n conversion specifier applies to a pointer to a signed integer type corresponding to a `size_t` argument.

**t**
Specifies that a following d, i, o, u, x, or X conversion specifier applies to a `ptrdiff_t` or the corresponding unsigned type argument; or that a following n conversion specifier applies to a pointer to a `ptrdiff_t` argument.

**L**
Specifies that a following a, A, e, E, f, F, g, or G conversion specifier applies to a `long double` argument.

If a length modifier appears with any conversion specifier other than as specified above, the behavior is undefined.

### Conversion Specifiers

The conversion specifiers and their meanings are:

**d, i**
The `int` argument shall be converted to a signed decimal in the style "[-]dddd". The precision specifies the minimum number of digits to appear; if the value being converted can be represented in fewer digits, it shall be expanded with leading zeros. The default precision is 1. The result of converting zero with an explicit precision of zero shall be no characters.

**o**
The `unsigned` argument shall be converted to unsigned octal format in the style "dddd". The precision specifies the minimum number of digits to appear; if the value being converted can be represented in fewer digits, it shall be expanded with leading zeros. The default precision is 1. The result of converting zero with an explicit precision of zero shall be no characters.

**u**
The `unsigned` argument shall be converted to unsigned decimal format in the style "dddd". The precision specifies the minimum number of digits to appear; if the value being converted can be represented in fewer digits, it shall be expanded with leading zeros. The default precision is 1. The result of converting zero with an explicit precision of zero shall be no characters.

**x**
The `unsigned` argument shall be converted to unsigned hexadecimal format in the style "dddd"; the letters "abcdef" are used. The precision specifies the minimum number of digits to appear; if the value being converted can be represented in fewer digits, it shall be expanded with leading zeros. The default precision is 1. The result of converting zero with an explicit precision of zero shall be no characters.

**X**
Equivalent to the x conversion specifier, except that letters "ABCDEF" are used instead of "abcdef".

**f, F**
The `double` argument shall be converted to decimal notation in the style "[-]ddd.ddd", where the number of digits after the radix character is equal to the precision specification. If the precision is missing, it shall be taken as 6; if the precision is explicitly zero and no '#' flag is present, no radix character shall appear. If a radix character appears, at least one digit appears before it. The low-order digit shall be rounded in an implementation-defined manner.

A `double` argument representing an infinity shall be converted in one of the styles "[-]inf" or "[-]infinity"; which style is implementation-defined. A `double` argument representing a NaN shall be converted in one of the styles "[-]nan(n-char-sequence)" or "[-]nan"; which style, and the meaning of any n-char-sequence, is implementation-defined. The F conversion specifier produces "INF", "INFINITY", or "NAN" instead of "inf", "infinity", or "nan", respectively.

**e, E**
The `double` argument shall be converted in the style "[-]d.ddd e±dd", where there is one digit before the radix character (which is non-zero if the argument is non-zero) and the number of digits after it is equal to the precision; if the precision is missing, it shall be taken as 6; if the precision is zero and no '#' flag is present, no radix character shall appear. The low-order digit shall be rounded in an implementation-defined manner. The E conversion specifier shall produce a number with 'E' instead of 'e' introducing the exponent. The exponent shall always contain at least two digits. If the value is zero, the exponent shall be zero.

A `double` argument representing an infinity or NaN shall be converted in the style of an f or F conversion specifier.

**g, G**
The `double` argument representing a floating-point number shall be converted in the style f or e (or in the style F or E in the case of a G conversion specifier), depending on the value converted and the precision. Let P equal the precision if non-zero, 6 if the precision is omitted, or 1 if the precision is zero. Then, if a conversion with style E would have an exponent of X:

* If P > X >= -4, the conversion shall be with style f (or F) and precision P-(X+1).
* Otherwise, the conversion shall be with style e (or E) and precision P-1.

Finally, unless the '#' flag is used, any trailing zeros shall be removed from the fractional portion of the result and the decimal-point character shall be removed if there is no fractional portion remaining.

A `double` argument representing an infinity or NaN shall be converted in the style of an f or F conversion specifier.

**a, A**
A `double` argument representing a floating-point number shall be converted in the style "[-]0x h.hhhhp±d", where there is one hexadecimal digit (which is non-zero if the argument is a normalized floating-point number and is otherwise unspecified) before the radix character and the number of hexadecimal digits after it is equal to the precision; if the precision is missing, it shall be taken as sufficient to represent the floating-point number exactly, and if the precision is zero and no '#' flag is present, no radix character shall appear. The letters "abcdef" are used for a conversion, the letters "ABCDEF" for A conversion. The A conversion specifier produces a number with 'X' and 'P' instead of 'x' and 'p'. The exponent shall always contain at least one digit, and only as many more digits as necessary to represent the decimal exponent of 2.

A `double` argument representing an infinity or NaN shall be converted in the style of an f or F conversion specifier.

**c**
If no l length modifier is present, the `int` argument shall be converted to an `unsigned char`, and the resulting byte shall be written.

If an l length modifier is present, the `wint_t` argument shall be converted as if by a call to `wcrtomb()` with no state, and the resulting multibyte character shall be written.

**s**
If no l length modifier is present, the application shall ensure that the argument is a pointer to a character array containing a character sequence beginning in the initial shift state. Characters from the array shall be written up to (but not including) the terminating null byte. If the precision is specified, no more than that many bytes shall be written and any partial multibyte character shall not be written. If the precision is not specified or is greater than the size of the array, the application shall ensure that the array contains a null byte.

If an l length modifier is present, the application shall ensure that the argument is a pointer to an array of type `wchar_t`. Wide characters from the array shall be converted to multibyte characters (each as if by a call to `wcrtomb()`, with the conversion state described by an mbstate_t object initialized to zero before the first wide character is converted) and the resulting multibyte characters shall be written up to (but not including) the terminating null wide character. If the precision is specified, no more than that many bytes shall be written and any partial multibyte character shall not be written. If the precision is not specified or is greater than the size of the converted wide character sequence, the application shall ensure that the array contains a null wide character.

**p**
The application shall ensure that the argument is a pointer to `void`. The value of the pointer is converted to a sequence of printable characters, in an implementation-defined manner.

**n**
The application shall ensure that the argument is a pointer to an integer into which is written the number of bytes written to the output so far by this call to one of the `fprintf()` functions. No argument is converted. The format string shall be written to the output as-is; if a conversion specification contains a field width or precision, the value of the bytes written to the output shall be determined as if those values had been specified.

For `snprintf()`, this means the number of bytes that would have been written to the buffer s, not counting the terminating null byte, if n were large enough.

**%**
A '%' character shall be written. No argument is converted. The complete conversion specification shall be "%%".

If a conversion specification does not match one of the above forms, the behavior is undefined.

## RETURN VALUE

Upon successful completion, the `fprintf()`, `printf()`, `dprintf()`, `snprintf()`, `asprintf()`, and `sprintf()` functions shall return the number of bytes transmitted.

If an output error was encountered, these functions shall return a negative value.

If the value of n is zero on a call to `snprintf()`, nothing shall be written, the number of bytes that would have been written had n been sufficiently large shall be returned, and s may be a null pointer.

## ERRORS

The functions shall fail if:

**EILSEQ**
An illegal byte sequence was detected in a wide character conversion.

**EOVERFLOW**
The value to be stored is larger than an integer of the corresponding type.

The `fprintf()`, `printf()`, `dprintf()`, `snprintf()`, and `sprintf()` functions may fail if:

**ENOMEM**
Insufficient storage space is available.

The `snprintf()` function shall fail if:

**EOVERFLOW**
The value of n is greater than {INT_MAX} or insufficient storage space was provided.

The `asprintf()` function shall fail if:

**ENOMEM**
Insufficient memory space is available to store the resulting string.

The `dprintf()` function shall fail if:

**EBADF**
The file descriptor underlying the stream is not a valid file descriptor or is not open for writing.

The `fprintf()` and `printf()` functions may fail if:

**ENOMEM**
Insufficient storage space is available.

**EILSEQ**
A wide-character code that does not correspond to a valid character has been detected.

## EXAMPLES

### Example 1: Print language-independent date and time

The following statement can be used to print date and time in a language-independent way:

```c
printf (format, weekday, month, day, hour, min);
```

For American usage, `format` could be a pointer to the following string:

```c
"%s, %s %d, %.2d:%.2d\n"
```

This example could produce the following output:

```
Sunday, July 3, 10:02
```

For German usage, `format` could be a pointer to the following string:

```c
"%1$s, %3$d. %2$s, %4$02.2d:%5$02.2d\n"
```

This definition of `format` could produce the following output:

```
Sonntag, 3. Juli, 10:02
```

### Example 2: Print a file list

To print a date and time in the form Sunday, July 3, 10:02, where weekday and month are pointers to null-terminated strings:

```c
printf("%s, %s %d, %.2d:%.2d\n", weekday, month, day, hour, min);
```

To print pi to 5 decimal places:

```c
printf("pi = %.5f\n", 3.1415926535);
```

The above example could produce the following output:

```
pi = 3.14159
```

### Example 3: Print to different destinations

```c
#include <stdio.h>
#include <stdlib.h>

int main(void) {
    char buffer[100];
    char *dynamic_buffer = NULL;
    int ret;

    /* Write to standard output */
    ret = printf("Hello, World!\n");

    /* Write to buffer */
    ret = sprintf(buffer, "Count: %d", 42);

    /* Write to buffer with size limit */
    ret = snprintf(buffer, sizeof(buffer), "Value: %f", 3.14159);

    /* Write to dynamically allocated buffer */
    ret = asprintf(&dynamic_buffer, "String: %s", "Dynamic");
    if (ret != -1) {
        printf("Allocated: %s\n", dynamic_buffer);
        free(dynamic_buffer);
    }

    return 0;
}
```

## APPLICATION USAGE

Before calling `asprintf()`, the value of `*ptr` is indeterminate and should not be assumed to be `NULL`. If the call is successful, the application shall free the memory allocated by `asprintf()` using `free()`.

## RATIONALE

The `sprintf()` function is prone to buffer overruns. The `snprintf()` function was created to address this security concern by limiting the number of bytes written to the buffer.

The `asprintf()` function was added to handle the common case where the programmer does not know in advance how large a buffer needs to be for the formatted output.

The numbered argument specifications ("%n$") provide support for languages where word order differs from English, allowing format strings to be reordered without changing the argument order in function calls.

## FUTURE DIRECTIONS

None.

## SEE ALSO

`fclose()`, `fopen()`, `fputc()`, `puts()`, `scanf()`, `wcrtomb()`

---
