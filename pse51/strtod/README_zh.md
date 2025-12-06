# strtod, strtof, strtold — 将字符串转换为双精度浮点数

## 概要

```c
#include <stdlib.h>

double strtod(const char *restrict nptr, char **restrict endptr);
float strtof(const char *restrict nptr, char **restrict endptr);
long double strtold(const char *restrict nptr, char **restrict endptr);
```

## 描述

[CX] 本参考页描述的功能与ISO C标准保持一致。此处描述的要求与ISO C标准之间的冲突是无意的。本卷POSIX.1-2024遵循ISO C标准。

这些函数应将`nptr`指向的字符串的初始部分分别转换为`double`、`float`和`long double`表示。首先，它们将输入字符串分解为三个部分：

1. 一个初始的、可能为空的空白字节序列
2. 一个被解释为浮点常量或表示无穷大或NaN的主题序列
3. 由一个或多个未识别字符组成的最终字符串，包括输入字符串的终止NUL字符

然后它们应尝试将主题序列转换为浮点数，并返回结果。

主题序列的预期形式是一个可选的'+'或'-'符号，然后是以下形式之一：

- 一个非空的十进制数字序列，可选择包含基数字符；然后是��个可选的指数部分，由字符'e'或字符'E'组成，可选择后跟'+'或'-'字符，然后再后跟一个或多个十进制数字
- 一个0x或0X，然后是一个非空的十六进制数字序列，可选择包含基数字符；然后是一个可选的二进制指数部分，由字符'p'或字符'P'组成，可选择后跟'+'或'-'字符，然后再后跟一个或多个十进制数字
- INF或INFINITY之一，忽略大小写
- NAN或NAN(n-char-sequenceopt)之一，NAN部分忽略大小写，其中：

```
n-char-sequence:
    digit
    nondigit
    n-char-sequence digit
    n-char-sequence nondigit
```

主题序列定义为输入字符串的最长初始子序列，从第一个非空白字节开始，具有预期形式。如果输入字符串不具有预期形式，则主题序列不包含任何字符。

如果主题序列具有浮点数的预期形式，从第一个数字或小数点字符（以先出现的为准）开始的字符序列应被解释为C语言的浮点常量，除了基数字符应代替句点使用，并且如果在十进制浮点数中既没有指数部分也没有基数字符，或者在十六进制浮点数中没有二进制指数部分，则假定值零的适当类型指数部分跟在字符串中的最后一个数字之后。如果主题序列以<hyphen-minus>开始，该序列应被解释为否定。字符序列INF或INFINITY应被解释为无穷大，如果在返回类型中可表示，否则应被解释为对于返回类型范围来说过大的浮点常量。字符序列NAN或NAN(n-char-sequenceopt)应被解释为安静NaN，如果在返回类型中支持，否则应被解释为不具有预期形式的主题序列部分；n-char序列的含义是实现定义的。指向最终字符串的指针存储在`endptr`指向的对象中，前提是`endptr`不是空指针。

如果主题序列具有十六进制形式且FLT_RADIX是2的幂，则转换产生的值被正确舍入。

[CX] 基数字符在当前语言环境（类别LC_NUMERIC）中定义。在POSIX语言环境或基数字符未定义的语言环境中，基数字符应默认为<period>（'.'）。

在C [CX]或POSIX语言环境之外，可以接受其他特定于语言环境的主题序列形式。

如果主题序列为空或不具有预期形式，则不执行转换；`nptr`的值存储在`endptr`指向的对象中，前提是`endptr`不是空指针。

如果成功，这些函数不应更改`errno`的设置。

由于错误时返回0，成功时也是有效的返回值，希望检查错误情况的应用程序应将`errno`设置为0，然后调用`strtod()`、`strtof()`或`strtold()`，然后检查`errno`。

## 返回值

成功完成时，这些函数应返回转换后的值。如果无法执行转换，应返回0，并且`errno`可以设置为[EINVAL]。

如果正确值会导致溢出且默认舍入生效，应返回±HUGE_VAL、±HUGE_VALF或±HUGE_VALL（根据值的符号），并且`errno`应设置为[ERANGE]。

如果正确值会导致下溢，应返回其大小不大于返回类型中最小规范化正数的值[CX]，并且`errno`设置为[ERANGE]。

## 错误

这些函数在以下情况下可能失败：

- **[ERANGE]** 要返回的值会导致溢出且默认舍入生效[CX]，或者要返回的值会导致下溢。

这些函数在以下情况下可能失败：

- **[EINVAL]** [CX] 无法执行转换。

## 示例

无。

## 应用程序用法

如果主题序列具有十六进制形式且FLT_RADIX不是2的幂，且结果不能精确表示，结果应该是适当内部格式中与十六进制浮点源值相邻的两个数字之一，额外要求误差对于当前舍入方向应该具有正确的符号。

如果主题序列具有十进制形式且最多有DECIMAL_DIG（在<float.h>中定义）个有效数字，结果应该被正确舍入。如果主题序列D具有十进制形式且有超过DECIMAL_DIG个有效数字，考虑两个边界相邻的十进制字符串L和U，两者都具有DECIMAL_DIG个有效数字，使得L、D和U的值满足L <= D <= U。结果应该是通过根据当前舍入方向正确舍入L和U获得的（相等或相邻）值之一，额外要求相对于D的误差对于当前舍入方向应该具有正确的符号。

ISO/IEC 9899:1999标准引入的`strtod()`更改可能改变符合ISO/IEC 9899:1990标准及本标准早期版本的良好格式应用程序的行为。一个这样的例子是：

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

如果使用以下方式调用函数：

```c
what_kind_of_number ("0x10")
```

符合ISO/IEC 9899:1990标准的库将导致函数打印：

```
It's an integer with value 16
```

使用ISO/IEC 9899:1999标准，结果是：

```
It's a float with value 16
```

行为的变化是由于包含了十六进制表示法的浮点数，而不要求小数点或二进制指数存在。

## 原理

无。

## 未来方向

无。

## 另请参阅

- `fscanf()`
- `isspace()`
- `localeconv()`
- `setlocale()`
- `strtol()`
- XBD [7. 语言环境](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/V1_chap07.html#tag_07)
- `<float.h>`
- `<stdlib.h>`

## 变更历史

### 首次发布于Issue 1
派生自SVID的Issue 1。

### Issue 5
描述更新为指示如果函数成功则不更改`errno`。

### Issue 6
超出ISO C标准的扩展被标记。

以下对POSIX实现的新要求源于与单一UNIX规范的对齐：

- 在返回值和错误部分，如果无法执行转换，则添加[EINVAL]可选错误条件。

为与ISO/IEC 9899:1999标准对齐进行以下更改：

- `strtod()`函数更新。
- 添加`strtof()`和`strtold()`函数。
- 描述广泛修订。

纳入ISO/IEC 9899:1999标准，技术勘误1。

应用IEEE Std 1003.1-2001/Cor 1-2002，项目XSH/TC1/D6/61，修正返回值部分的第二段。此更改阐明了返回值的符号。

### Issue 7
应用Austin Group解释1003.1-2001 #015。

应用POSIX.1-2008，技术勘误1，XSH/TC1-2008/0610 [302]，XSH/TC1-2008/0611 [94]，和XSH/TC1-2008/0612 [105]。

应用POSIX.1-2008，技术勘误2，XSH/TC2-2008/0348 [584]和XSH/TC2-2008/0349 [796]。

### Issue 8
应用Austin Group缺陷1163，阐明输入字符串中空白字符的处理。

应用Austin Group缺陷1213，修正应用程序用法部分中的一些印刷错误。

应用Austin Group缺陷1302，使这些函数与ISO/IEC 9899:2018标准对齐。

应用Austin Group缺陷1686，为返回值部分中的一些文本添加CX阴影。

---
