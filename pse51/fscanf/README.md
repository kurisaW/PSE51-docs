# fscanf

## Name

fscanf - convert formatted input

## Synopsis

```c
#include <stdio.h>

int fscanf(FILE *restrict stream, const char *restrict format, ...);
int scanf(const char *restrict format, ...);
int sscanf(const char *restrict s, const char *restrict format, ...);
```

## Description

[CX] The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

The `fscanf()` function shall read from the named input stream. The `scanf()` function shall read from the standard input stream stdin. The `sscanf()` function shall read from the string `s`. Each function reads bytes, interprets them according to a format, and stores the results in its arguments. Each expects, as arguments, a control string format described below, and a set of pointer arguments indicating where the converted input should be stored. The result is undefined if there are insufficient arguments for the format. If the format is exhausted while arguments remain, the excess arguments shall be evaluated but otherwise ignored.

[CX] Conversions can be applied to the n-th argument after the format in the argument list, rather than to the next unused argument. In this case, the conversion specifier character % (see below) is replaced by the sequence "%n$", where n is a decimal integer in the range [1,{NL_ARGMAX}]. This feature provides for the definition of format strings that select arguments in an order appropriate to specific languages. In format strings containing the "%n$" form of conversion specifications, it is unspecified whether numbered arguments in the argument list can be referenced from the format string more than once.

The format can contain either form of a conversion specification—that is, % or "%n$"—but the two forms cannot be mixed within a single format string. The only exception to this is that %% or %* can be mixed with the "%n$" form. When numbered argument specifications are used, specifying the N-th argument requires that all the leading arguments, from the first to the (N-1)th, are pointers.

The `fscanf()` function in all its forms shall allow detection of a language-dependent radix character in the input string. The radix character is defined in the current locale (category LC_NUMERIC). In the POSIX locale, or in a locale where the radix character is not defined, the radix character shall default to a period ('.').

### Format String

The application shall ensure that the format is a character string, beginning and ending in its initial shift state, if any, composed of zero or more directives. Each directive is composed of one of the following: one or more white-space bytes; an ordinary character (neither '%' nor a white-space byte); or a conversion specification. Each conversion specification is introduced by the character '%' [CX] or the character sequence "%n$", after which the following appear in sequence:

* An optional assignment-suppressing character '*'.
* An optional non-zero decimal integer that specifies the maximum field width.
* [CX] An optional assignment-allocation character 'm'.
* An optional length modifier that specifies the size of the receiving object.
* A conversion specifier character that specifies the type of conversion to be applied.

The `fscanf()` functions shall execute each directive of the format in turn. When all directives have been executed, or if a directive fails (as detailed below), the function shall return. Failures are described as input failures (due to the unavailability of input bytes) or matching failures (due to inappropriate input).

### Directive Execution

A directive composed of one or more white-space bytes shall be executed by reading input up to the first non-white-space byte, which shall remain unread, or until no more bytes can be read. The directive shall never fail.

A directive that is an ordinary character shall be executed as follows: the next byte shall be read from the input and compared with the byte that comprises the directive; if the comparison shows that they are not equivalent, the directive shall fail, and the differing and subsequent bytes shall remain unread. Similarly, if end-of-file, an encoding error, or a read error prevents a character from being read, the directive shall fail.

A directive that is a conversion specification defines a set of matching input sequences, as described below for each conversion character. A conversion specification shall be executed in the following steps:

1. Input white-space bytes shall be skipped, unless the conversion specification includes a [, c, C, or n conversion specifier.
2. An item shall be read from the input, unless the conversion specification includes an n conversion specifier. An input item shall be defined as the longest sequence of input bytes (up to any specified maximum field width, which may be measured in characters or bytes dependent on the conversion specifier) which is an initial subsequence of a matching sequence. The first byte, if any, after the input item shall remain unread. If the length of the input item is 0, the execution of the conversion specification shall fail; this condition is a matching failure, unless end-of-file, an encoding error, or a read error prevented input from the stream, in which case it is an input failure.
3. Except in the case of a % conversion specifier, the input item (or, in the case of a %n conversion specification, the count of input bytes) shall be converted to a type appropriate to the conversion character. If the input item is not a matching sequence, the execution of the conversion specification fails; this condition is a matching failure. Unless assignment suppression was indicated by a '*', the result of the conversion shall be placed in the object pointed to by the first argument following the format argument that has not already received a conversion result if the conversion specification is introduced by %, [CX] or in the n-th argument if introduced by the character sequence "%n$". If this object does not have an appropriate type, or if the result of the conversion cannot be represented in the space provided, the behavior is undefined.

[CX] The c, s, and [ conversion specifiers shall accept an optional assignment-allocation character 'm', which shall cause a memory buffer to be allocated to hold the conversion results. If the conversion specifier is s or [, the allocated buffer shall include space for a terminating null character (or wide character). In such a case, the argument corresponding to the conversion specifier should be a reference to a pointer variable that will receive a pointer to the allocated buffer. The system shall allocate a buffer as if `malloc()` had been called. The application shall be responsible for freeing the memory after usage. If there is insufficient memory to allocate a buffer, the function shall set errno to [ENOMEM] and a conversion error shall result. If the function returns EOF, any memory successfully allocated for parameters using assignment-allocation character 'm' by this call shall be freed before the function returns.

### Length Modifiers

The length modifiers and their meanings are:

**hh**
Specifies that a following d, i, o, u, x, X, or n conversion specifier applies to an argument with type pointer to `signed char` or `unsigned char`.

**h**
Specifies that a following d, i, o, u, x, X, or n conversion specifier applies to an argument with type pointer to `short` or `unsigned short`.

**l** (ell)
Specifies that a following d, i, o, u, x, X, or n conversion specifier applies to an argument with type pointer to `long` or `unsigned long`; that a following a, A, e, E, f, F, g, or G conversion specifier applies to an argument with type pointer to `double`; or that a following c, s, or [ conversion specifier applies to an argument with type pointer to `wchar_t`. [CX] If the 'm' assignment-allocation character is specified, the conversion applies to an argument with the type pointer to a pointer to `wchar_t`.

**ll** (ell-ell)
Specifies that a following d, i, o, u, x, X, or n conversion specifier applies to an argument with type pointer to `long long` or `unsigned long long`.

**j**
Specifies that a following d, i, o, u, x, X, or n conversion specifier applies to an argument with type pointer to `intmax_t` or `uintmax_t`.

**z**
Specifies that a following d, i, o, u, x, X, or n conversion specifier applies to an argument with type pointer to `size_t` or the corresponding signed integer type.

**t**
Specifies that a following d, i, o, u, x, X, or n conversion specifier applies to an argument with type pointer to `ptrdiff_t` or the corresponding `unsigned` type.

**L**
Specifies that a following a, A, e, E, f, F, g, or G conversion specifier applies to an argument with type pointer to `long double`.

If a length modifier appears with any conversion specifier other than as specified above, the behavior is undefined.

### Conversion Specifiers

The following conversion specifiers are valid:

**d**
Matches an optionally signed decimal integer, whose format is the same as expected for the subject sequence of `strtol()` with the value 10 for the base argument. In the absence of a size modifier, the application shall ensure that the corresponding argument is a pointer to `int`.

**i**
Matches an optionally signed integer, whose format is the same as expected for the subject sequence of `strtol()` with 0 for the base argument. In the absence of a size modifier, the application shall ensure that the corresponding argument is a pointer to `int`.

**o**
Matches an optionally signed octal integer, whose format is the same as expected for the subject sequence of `strtoul()` with the value 8 for the base argument. In the absence of a size modifier, the application shall ensure that the corresponding argument is a pointer to `unsigned`.

**u**
Matches an optionally signed decimal integer, whose format is the same as expected for the subject sequence of `strtoul()` with the value 10 for the base argument. In the absence of a size modifier, the application shall ensure that the corresponding argument is a pointer to `unsigned`.

**x**
Matches an optionally signed hexadecimal integer, whose format is the same as expected for the subject sequence of `strtoul()` with the value 16 for the base argument. In the absence of a size modifier, the application shall ensure that the corresponding argument is a pointer to `unsigned`.

**a, e, f, g**
Matches an optionally signed floating-point number, infinity, or NaN, whose format is the same as expected for the subject sequence of `strtod()`. In the absence of a size modifier, the application shall ensure that the corresponding argument is a pointer to `float`.

If the `fprintf()` family of functions generates character string representations for infinity and NaN (a symbolic entity encoded in floating-point format) to support IEEE Std 754-1985, the `fscanf()` family of functions shall recognize them as input.

**s**
Matches a sequence of bytes that are not white-space bytes. If the 'm' assignment-allocation character is not specified, the application shall ensure that the corresponding argument is a pointer to the initial byte of an array of `char`, `signed char`, or `unsigned char` large enough to accept the sequence and a terminating null character code, which shall be added automatically. [CX] Otherwise, the application shall ensure that the corresponding argument is a pointer to a pointer to a `char`.

If an l (ell) qualifier is present, the input is a sequence of characters that begins in the initial shift state. Each character shall be converted to a wide character as if by a call to the `mbrtowc()` function, with the conversion state described by an `mbstate_t` object initialized to zero before the first character is converted. If the 'm' assignment-allocation character is not specified, the application shall ensure that the corresponding argument is a pointer to an array of `wchar_t` large enough to accept the sequence and the terminating null wide character, which shall be added automatically. [CX] Otherwise, the application shall ensure that the corresponding argument is a pointer to a pointer to a `wchar_t`.

**[**
Matches a non-empty sequence of bytes from a set of expected bytes (the scanset). The normal skip over white-space bytes shall be suppressed in this case. If the 'm' assignment-allocation character is not specified, the application shall ensure that the corresponding argument is a pointer to the initial byte of an array of `char`, `signed char`, or `unsigned char` large enough to accept the sequence and a terminating null byte, which shall be added automatically. [CX] Otherwise, the application shall ensure that the corresponding argument is a pointer to a pointer to a `char`.

If an l (ell) qualifier is present, the input is a sequence of characters that begins in the initial shift state. Each character in the sequence shall be converted to a wide character as if by a call to the `mbrtowc()` function, with the conversion state described by an `mbstate_t` object initialized to zero before the first character is converted. If the 'm' assignment-allocation character is not specified, the application shall ensure that the corresponding argument is a pointer to an array of `wchar_t` large enough to accept the sequence and the terminating null wide character, which shall be added automatically. [CX] Otherwise, the application shall ensure that the corresponding argument is a pointer to a pointer to a `wchar_t`.

The conversion specification includes all subsequent bytes in the format string up to and including the matching right-square-bracket (']'). The bytes between the square brackets (the scanlist) comprise the scanset, unless the byte after the left-square-bracket is a circumflex ('^'), in which case the scanset contains all bytes that do not appear in the scanlist between the circumflex and the right-square-bracket. If the conversion specification begins with "[]" or "[^]", the right-square-bracket is included in the scanlist and the next right-square-bracket is the matching right-square-bracket that ends the conversion specification; otherwise, the first right-square-bracket is the one that ends the conversion specification. If a '-' is in the scanlist and is not the first character, nor the second where the first character is a '^', nor the last character, the behavior is implementation-defined.

**c**
Matches a sequence of bytes of the number specified by the field width (1 if no field width is present in the conversion specification). No null byte is added. The normal skip over white-space bytes shall be suppressed in this case. If the 'm' assignment-allocation character is not specified, the application shall ensure that the corresponding argument is a pointer to the initial byte of an array of `char`, `signed char`, or `unsigned char` large enough to accept the sequence. [CX] Otherwise, the application shall ensure that the corresponding argument is a pointer to a pointer to a `char`.

If an l (ell) qualifier is present, the input shall be a sequence of characters that begins in the initial shift state. Each character in the sequence is converted to a wide character as if by a call to the `mbrtowc()` function, with the conversion state described by an `mbstate_t` object initialized to zero before the first character is converted. No null wide character is added. If the 'm' assignment-allocation character is not specified, the application shall ensure that the corresponding argument is a pointer to an array of `wchar_t` large enough to accept the resulting sequence of wide characters. [CX] Otherwise, the application shall ensure that the corresponding argument is a pointer to a pointer to a `wchar_t`.

**p**
Matches an implementation-defined set of sequences, which shall be the same as the set of sequences that is produced by the %p conversion specification of the corresponding `fprintf()` functions. The application shall ensure that the corresponding argument is a pointer to a pointer to `void`. The interpretation of the input item is implementation-defined. If the input item is a value converted earlier during the same program execution, the pointer that results shall compare equal to that value; otherwise, the behavior of the %p conversion specification is undefined.

**n**
No input is consumed. The application shall ensure that the corresponding argument is a pointer to the integer into which shall be written the number of bytes read from the input so far by this call to the `fscanf()` functions. Execution of a %n conversion specification shall not increment the assignment count returned at the completion of execution of the function. No argument shall be converted, but one shall be consumed.

**%**
Matches a single '%' character; no assignment or conversion shall be done. The complete conversion specification shall be %%.

## Return Value

Upon successful completion, these functions shall return the number of successfully matched and assigned input items; this number can be zero in the event of an early matching failure. If the input ends before the first matching failure or conversion, EOF shall be returned. If a read error occurs, the error indicator for the stream shall be set, EOF shall be returned, and `errno` shall be set to indicate the error.

## Errors

For the conditions under which the functions shall fail and may fail, see the documentation for the underlying `fgetc()` or `fgetwc()` functions. In addition, the `fscanf()` functions may fail if:

* **[ENOMEM]** - Insufficient storage space is available to hold the converted value when using the 'm' assignment-allocation character.

## Examples

```c
#include <stdio.h>

int main(void) {
    int i;
    float f;
    char s[100];

    /* Read integer, float, and string */
    if (fscanf(stdin, "%d %f %99s", &i, &f, s) == 3) {
        printf("Read: %d, %f, %s\n", i, f, s);
    }

    return 0;
}
```

## Application Usage

The `fscanf()` family of functions may be used in applications that need to read and parse formatted input from files, standard input, or strings. The format string provides flexible control over how input is interpreted and converted.

The assignment-allocation character 'm' is particularly useful when the input size is not known in advance, as it allows the implementation to allocate appropriate memory automatically.

## Rationale

The `fscanf()` functions provide a powerful mechanism for input parsing and conversion. The design allows for:

* Precise control over input parsing through format specifications
* Automatic memory allocation for variable-length input when using the 'm' modifier
* Support for both narrow and wide character input
* Numbered argument specifications for internationalized applications

The behavior is carefully specified to ensure consistent results across different implementations while maintaining compatibility with the ISO C standard.

## Future Directions

None.

## See Also

`fgetc()`, `fgetwc()`, `fprintf()`, `fread()`, `fseek()`, `getchar()`, `scanf()`, `setlocale()`, `sscanf()`, `strtod()`, `strtol()`, `strtoul()`, `ungetc()`