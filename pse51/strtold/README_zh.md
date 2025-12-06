# strtold

## SYNOPSIS

```c
#include <stdlib.h>

double strtod(const char *restrict nptr, char **restrict endptr);
float strtof(const char *restrict nptr, char **restrict endptr);
long double strtold(const char *restrict nptr, char **restrict endptr);
```

## DESCRIPTION

[CX] 本参考页描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是非故意的。POSIX.1-2024 标准遵从 ISO C 标准。

这些函数应将由 `nptr` 指向的字符串的初始部分分别转换为 **double**、**float** 和 **long double** 表示形式。首先，它们将输入字符串分解为三个部分：

1. 一个初始的、可能为空的空白字节序列
2. 一个解释为浮点常量或表示无穷大或 NaN 的主题序列
3. 一个或多个无法识别的字符的最终字符串，包括输入字符串的终止 NUL 字符

然后它们应尝试将主题序列转换为浮点数，并返回结果。

主题序列的预期形式是一个可选的 '+' 或 '-' 符号，然后是以下形式之一：

- 一个非空的十进制数字序列，可选包含基数字符；然后是一个可选的指数部分，由字符 'e' 或字符 'E' 组成，可选后跟 '+' 或 '-' 字符，然后再后跟一个或多个十进制数字
- 一个 0x 或 0X，然后是一个非空的十六进制数字序列，可选包含基数字符；然后是一个可选的二进制指数部分，由字符 'p' 或字符 'P' 组成，可选后跟 '+' 或 '-' 字符，然后再后跟一个或多个十进制数字
- INF 或 INFINITY 之一，忽略大小写
- NAN 或 NAN(`n-char-sequence`_opt_) 之一，在 NAN 部分忽略大小写，其中：

```
n-char-sequence:
    digit
    nondigit
    n-char-sequence digit
    n-char-sequence nondigit
```

主题序列定义为输入字符串的最长子序列，从第一个非空白字节开始，具有预期形式。如果输入字符串不具有预期形式，则主题序列不包含任何字符。

如果主题序列具有浮点数的预期形式，则从第一个数字或小数点字符（以先出现的为准）开始的字符序列应被解释为 C 语言的浮点常量，但应使用基数字符代替句点，并且如果在十进制浮点数中既没有指数部分也没有基数字符，或者在十六进制浮点数中没有二进制指数部分，则假定一个适当类型的值为零的指数部分跟在字符串中的最后一个数字之后。如果主题序列以 <连字符减号> 开头，则该序列应被解释为负数。字符序列 INF 或 INFINITY 应被解释为无穷大，如果在返回类型中可表示；否则应被解释为对于返回类型范围而言过大的浮点常量。字符序列 NAN 或 NAN(`n-char-sequence`_opt_) 应被解释为静态 NaN，如果在返回类型中支持；否则应被解释为不具有预期形式的主题序列部分；`n`-char 序列的含义由实现定义。指向最终字符串的指针存储在由 `endptr` 指向的对象中，前提是 `endptr` 不是空指针。

如果主题序列具有十六进制形式且 FLT_RADIX 是 2 的幂，则转换得到的值是正确舍入的。

[CX] 基数字符在当前区域设置（类别 LC_NUMERIC）中定义。在 POSIX 区域设置中，或在未定义基数字符的区域设置中，基数字符应默认为 <句点> ('.')。

除了 C [CX] 或 POSIX 区域设置之外，可以接受其他特定区域设置的主题序列形式。

如果主题序列为空或不具有预期形式，则不执行任何转换；`nptr` 的值存储在由 `endptr` 指向的对象中，前提是 `endptr` 不是空指针。

如果成功，这些函数不应改变 `errno` 的设置。

由于错误时返回 0，成功时也是有效的返回值，希望检查错误情况的应用程序应将 `errno` 设置为 0，然后调用 `strtod()`、`strtof()` 或 `strtold()`，然后检查 `errno`。

## RETURN VALUE

成功完成后，这些函数应返回转换后的值。如果无法执行转换，应返回 0，并可能将 `errno` 设置为 [EINVAL]。

如果正确值会导致溢出且默认舍入生效，应返回 ±HUGE_VAL、±HUGE_VALF 或 ±HUGE_VALL（根据值的符号），并将 `errno` 设置为 [ERANGE]。

如果正确值会导致下溢，应返回其大小不大于返回类型中最小规范化正数的值 [CX]，并将 `errno` 设置为 [ERANGE]。

## ERRORS

这些函数在以下情况下应失败：

- **[ERANGE]**: 要返回的值会导致溢出且默认舍入生效 [CX]，或要返回的值会导致下溢。

这些函数在以下情况下可能失败：

- **[EINVAL]**: [CX] 无法执行转换。

---

*以下章节为信息性内容。*

## EXAMPLES

无。

## APPLICATION USAGE

如果主题序列具有十六进制形式且 FLT_RADIX 不是 2 的幂，且结果不能精确表示，结果应是以十六进制浮点源值为相邻的适当内部格式中的两个数之一，额外规定误差对于当前舍入方向应具有正确的符号。

如果主题序列具有十进制形式且最多有 DECIMAL_DIG（在 `<float.h>` 中定义）个有效数字，结果应正确舍入。如果主题序列 D 具有十进制形式且有效数字超过 DECIMAL_DIG，考虑两个边界相邻十进制字符串 L 和 U，两者都具有 DECIMAL_DIG 个有效数字，使得 L、D 和 U 的值满足 L <= D <= U。结果应是通过根据当前舍入方向正确舍入 L 和 U 得到的（相等或相邻）值之一，额外规定相对于 D 的误差对于当前舍入方向应具有正确的符号。

ISO/IEC 9899:1999 标准对 `strtod()` 的引入可能改变符合 ISO/IEC 9899:1990 标准的格式良好应用程序的行为，因此也改变本标准的早期版本。一个这样的例子是：

```c
int
what_kind_of_number (char *s)
{
    char *endp;
    double d;
    long l;

    d = strtod(s, &endp);
    if (s != endp && *endp == '\0')
        printf("It's a float with value %g\n", d);
    else
    {
        l = strtol(s, &endp, 0);
        if (s != endp && *endp == '\0')
            printf("It's an integer with value %ld\n", l);
        else
            return 1;
    }
    return 0;
}
```

如果函数使用以下方式调用：

```c
what_kind_of_number ("0x10")
```

符合 ISO/IEC 9899:1990 标准的库将导致函数打印：

```
It's an integer with value 16
```

使用 ISO/IEC 9899:1999 标准，结果是：

```
It's a float with value 16
```

行为的变化是由于包含十六进制表示法的浮点数，而不要求存在小数点或二进制指数。

## RATIONALE

无。

## FUTURE DIRECTIONS

无。

## SEE ALSO

- [`fscanf()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fscanf.html)
- [`isspace()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isspace.html)
- [`localeconv()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/localeconv.html)
- [`setlocale()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/setlocale.html)
- [`strtol()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/strtol.html)

XBD [7. Locale](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/V1_chap07.html#tag_07), [`<float.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/float.h.html), [`<stdlib.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdlib.h.html)

## CHANGE HISTORY

首次在 Issue 1 中发布。源自 SVID 的 Issue 1。

### Issue 5

更新 DESCRIPTION 以表明如果函数成功，`errno` 不会改变。

### Issue 6

标记超出 ISO C 标准的扩展。

以下对 POSIX 实现的新要求源自与单一 UNIX 规范的对齐：

- 在 RETURN VALUE 和 ERRORS 部分中，如果无法执行转换，添加 [EINVAL] 可选错误条件。

为与 ISO/IEC 9899:1999 标准对齐，进行以下更改：

- 更新 `strtod()` 函数。
- 添加 `strtof()` 和 `strtold()` 函数。
- 大幅修订 DESCRIPTION。

纳入 ISO/IEC 9899:1999 标准技术勘误 1。

应用 IEEE Std 1003.1-2001/Cor 1-2002，项目 XSH/TC1/D6/61，更正 RETURN VALUE 部分的第二段。此更改阐明了返回值的符号。

### Issue 7

应用 Austin Group Interpretation 1003.1-2001 #015。

应用 POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0610 [302], XSH/TC1-2008/0611 [94], 和 XSH/TC1-2008/0612 [105]。

应用 POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0348 [584] 和 XSH/TC2-2008/0349 [796]。

### Issue 8

应用 Austin Group Defect 1163，阐明输入字符串中空白字符的处理。

应用 Austin Group Defect 1213，更正 APPLICATION USAGE 部分中的一些印刷错误。

应用 Austin Group Defect 1302，将这些函数与 ISO/IEC 9899:2018 标准对齐。

应用 Austin Group Defect 1686，在 RETURN VALUE 部分的某些文本中添加 CX 底纹。

---
