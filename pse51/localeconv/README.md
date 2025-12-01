# localeconv — return locale-specific information

## SYNOPSIS

```c
#include <locale.h>

struct lconv *localeconv(void);
```

## DESCRIPTION

The `localeconv()` function shall set the components of an object with the type `struct lconv` with the values appropriate for the formatting of numeric quantities (monetary and otherwise) according to the rules of the current locale.

The members of the structure with type `char *` are pointers to strings, any of which (except `decimal_point`) can point to "", to indicate that the value is not available in the current locale or is of zero length. The members with type `char` are non-negative numbers, any of which can be {CHAR_MAX} to indicate that the value is not available in the current locale.

### Structure Members

**char *decimal_point**
- The radix character used to format non-monetary quantities.

**char *thousands_sep**
- The character used to separate groups of digits before the decimal-point character in formatted non-monetary quantities.

**char *grouping**
- A string whose elements taken as one-byte integer values indicate the size of each group of digits in formatted non-monetary quantities.

**char *int_curr_symbol**
- The international currency symbol applicable to the current locale. The first three characters contain the alphabetic international currency symbol in accordance with those specified in the ISO 4217:2015 standard. The fourth character (immediately preceding the null byte) is the character used to separate the international currency symbol from the monetary quantity.

**char *currency_symbol**
- The local currency symbol applicable to the current locale.

**char *mon_decimal_point**
- The radix character used to format monetary quantities.

**char *mon_thousands_sep**
- The separator for groups of digits before the decimal-point in formatted monetary quantities.

**char *mon_grouping**
- A string whose elements taken as one-byte integer values indicate the size of each group of digits in formatted monetary quantities.

**char *positive_sign**
- The string used to indicate a non-negative valued formatted monetary quantity.

**char *negative_sign**
- The string used to indicate a negative valued formatted monetary quantity.

**char int_frac_digits**
- The number of fractional digits (those after the decimal-point) to be displayed in an internationally formatted monetary quantity.

**char frac_digits**
- The number of fractional digits (those after the decimal-point) to be displayed in a formatted monetary quantity.

**char p_cs_precedes**
- Set to 1 if the `currency_symbol` precedes the value for a non-negative formatted monetary quantity. Set to 0 if the symbol succeeds the value.

**char p_sep_by_space**
- Set to a value indicating the separation of the `currency_symbol`, the sign string, and the value for a non-negative formatted monetary quantity.

**char n_cs_precedes**
- Set to 1 if the `currency_symbol` precedes the value for a negative formatted monetary quantity. Set to 0 if the symbol succeeds the value.

**char n_sep_by_space**
- Set to a value indicating the separation of the `currency_symbol`, the sign string, and the value for a negative formatted monetary quantity.

**char p_sign_posn**
- Set to a value indicating the positioning of the `positive_sign` for a non-negative formatted monetary quantity.

**char n_sign_posn**
- Set to a value indicating the positioning of the `negative_sign` for a negative formatted monetary quantity.

**char int_p_cs_precedes**
- Set to 1 or 0 if the `int_curr_symbol` respectively precedes or succeeds the value for a non-negative internationally formatted monetary quantity.

**char int_n_cs_precedes**
- Set to 1 or 0 if the `int_curr_symbol` respectively precedes or succeeds the value for a negative internationally formatted monetary quantity.

**char int_p_sep_by_space**
- Set to a value indicating the separation of the `int_curr_symbol`, the sign string, and the value for a non-negative internationally formatted monetary quantity.

**char int_n_sep_by_space**
- Set to a value indicating the separation of the `int_curr_symbol`, the sign string, and the value for a negative internationally formatted monetary quantity.

**char int_p_sign_posn**
- Set to a value indicating the positioning of the `positive_sign` for a non-negative internationally formatted monetary quantity.

**char int_n_sign_posn**
- Set to a value indicating the positioning of the `negative_sign` for a negative internationally formatted monetary quantity.

### Grouping Interpretation

The elements of `grouping` and `mon_grouping` are interpreted according to the following:

- **{CHAR_MAX}**: No further grouping is to be performed.
- **0**: The previous element is to be repeatedly used for the remainder of the digits.
- **other**: The integer value is the number of digits that comprise the current group. The next element is examined to determine the size of the next group of digits before the current group.

### Space Separation Values

The values of `p_sep_by_space`, `n_sep_by_space`, `int_p_sep_by_space`, and `int_n_sep_by_space` are interpreted according to the following:

- **0**: No space separates the currency symbol and value.
- **1**: If the currency symbol and sign string are adjacent, a space separates them from the value; otherwise, a space separates the currency symbol from the value.
- **2**: If the currency symbol and sign string are adjacent, a space separates them; otherwise, a space separates the sign string from the value.

For `int_p_sep_by_space` and `int_n_sep_by_space`, the fourth character of `int_curr_symbol` is used instead of a space.

### Sign Positioning Values

The values of `p_sign_posn`, `n_sign_posn`, `int_p_sign_posn`, and `int_n_sign_posn` are interpreted according to the following:

- **0**: Parentheses surround the quantity and `currency_symbol` or `int_curr_symbol`.
- **1**: The sign string precedes the quantity and `currency_symbol` or `int_curr_symbol`.
- **2**: The sign string succeeds the quantity and `currency_symbol` or `int_curr_symbol`.
- **3**: The sign string immediately precedes the `currency_symbol` or `int_curr_symbol`.
- **4**: The sign string immediately succeeds the `currency_symbol` or `int_curr_symbol`.

The implementation shall behave as if no function in this volume of POSIX.1-2024 calls `localeconv()`.

The `localeconv()` function need not be thread-safe; however, `localeconv()` shall avoid data races with all other functions.

## RETURN VALUE

The `localeconv()` function shall return a pointer to the filled-in object. The application shall not modify the structure to which the return value points, nor any storage areas pointed to by pointers within the structure. The returned pointer, and pointers within the structure, might be invalidated or the structure or the storage areas might be overwritten by a subsequent call to `localeconv()`. In addition, the returned pointer, and pointers within the structure, might be invalidated or the structure or the storage areas might be overwritten by subsequent calls to `setlocale()` with the categories LC_ALL, LC_MONETARY, or LC_NUMERIC, or by calls to `uselocale()` which change the categories LC_MONETARY or LC_NUMERIC. The returned pointer, pointers within the structure, the structure, and the storage areas might also be invalidated if the calling thread is terminated.

## ERRORS

No errors are defined.

## EXAMPLES

None.

## APPLICATION USAGE

The following table illustrates the rules which may be used by four countries to format monetary quantities:

| Country | Positive Format | Negative Format | International Format |
|---------|-----------------|-----------------|----------------------|
| Italy | €.1.230 | \-€.1.230 | EUR.1.230 |
| Netherlands | € 1.234,56 | € -1.234,56 | EUR 1.234,56 |
| Norway | kr1.234,56 | kr1.234,56- | NOK 1.234,56 |
| Switzerland | SFrs.1,234.56 | SFrs.1,234.56C | CHF 1,234.56 |

For these four countries, the respective values for the monetary members of the structure returned by `localeconv()` are:

| Member | Italy | Netherlands | Norway | Switzerland |
|--------|--------|-------------|--------|-------------|
| **int_curr_symbol** | "EUR." | "EUR " | "NOK " | "CHF " |
| **currency_symbol** | "€." | "€" | "kr" | "SFrs." |
| **mon_decimal_point** | "" | "," | "," | "." |
| **mon_thousands_sep** | "." | "." | "." | "," |
| **mon_grouping** | "\3" | "\3" | "\3" | "\3" |
| **positive_sign** | "" | "" | "" | "" |
| **negative_sign** | "-" | "-" | "-" | "C" |
| **int_frac_digits** | 0 | 2 | 2 | 2 |
| **frac_digits** | 0 | 2 | 2 | 2 |
| **p_cs_precedes** | 1 | 1 | 1 | 1 |
| **p_sep_by_space** | 0 | 1 | 0 | 0 |
| **n_cs_precedes** | 1 | 1 | 1 | 1 |
| **n_sep_by_space** | 0 | 1 | 0 | 0 |
| **p_sign_posn** | 1 | 1 | 1 | 1 |
| **n_sign_posn** | 1 | 4 | 2 | 2 |
| **int_p_cs_precedes** | 1 | 1 | 1 | 1 |
| **int_n_cs_precedes** | 1 | 1 | 1 | 1 |
| **int_p_sep_by_space** | 0 | 0 | 0 | 0 |
| **int_n_sep_by_space** | 0 | 0 | 0 | 0 |
| **int_p_sign_posn** | 1 | 1 | 1 | 1 |
| **int_n_sign_posn** | 1 | 4 | 4 | 2 |

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- [`fprintf()`](fprintf.html)
- [`fscanf()`](fscanf.html)
- [`isalpha()`](isalpha.html)
- [`nl_langinfo()`](nl_langinfo.html)
- [`setlocale()`](setlocale.html)
- [`strcat()`](strcat.html)
- [`strchr()`](strchr.html)
- [`strcmp()`](strcmp.html)
- [`strcoll()`](strcoll.html)
- [`strcpy()`](strcpy.html)
- [`strftime()`](strftime.html)
- [`strlen()`](strlen.html)
- [`strpbrk()`](strpbrk.html)
- [`strspn()`](strspn.html)
- [`strtok()`](strtok.html)
- [`strxfrm()`](strxfrm.html)
- [`strtod()`](strtod.html)
- [`uselocale()`](uselocale.html)
- XBD [`<langinfo.h>`](langinfo.h.html), [`<locale.h>`](locale.h.html)

## CHANGE HISTORY

### First released in Issue 4. Derived from the ANSI C standard.

### Issue 6

- A note indicating that this function need not be reentrant is added to the DESCRIPTION.
- The RETURN VALUE section is rewritten to avoid use of the term "must".
- This reference page is updated for alignment with the ISO/IEC 9899:1999 standard.
- ISO/IEC 9899:1999 standard, Technical Corrigendum 1 is incorporated.
- IEEE Std 1003.1-2001/Cor 1-2002, item XSH/TC1/D6/31 is applied, removing references to `int_curr_symbol` and updating the descriptions of `p_sep_by_space` and `n_sep_by_space`. These changes are for alignment with the ISO C standard.

### Issue 7

- Austin Group Interpretation 1003.1-2001 #156 is applied.
- The definitions of `int_curr_symbol` and `currency_symbol` are updated.
- The examples in the APPLICATION USAGE section are updated.
- POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0362 [75] is applied.
- POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0200 [656] is applied.

### Issue 8

- Austin Group Defect 1302 is applied, aligning this function with the ISO/IEC 9899:2018 standard.