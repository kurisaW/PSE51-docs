# localeconv — 返回本地化特定信息

## 概要

```c
#include <locale.h>

struct lconv *localeconv(void);
```

## 描述

`localeconv()` 函数应当根据当前本地化环境的规则，用适于格式化数值（货币和非货币）��值来设置 `struct lconv` 类型对象的各个成员。

类型为 `char *` 的结构体成员是指向字符串的指针，其中任何一个（除了 `decimal_point`）都可以指向 ""，表示在当前本地化环境中该值不可用或长度为零。类型为 `char` 的成员是非负数，其中任何一个都可以是 {CHAR_MAX}，表示在当前本地化环境中该值不可用。

### 结构体成员

**char *decimal_point**
- 用于格式化非货币数值的基数字符。

**char *thousands_sep**
- 在格式化的非货币数值中，用于分隔小数点前数字组的字符。

**char *grouping**
- 一个字符串，其元素被视为单字节整数值，表示格式化的非货币数值中每个数字组的大小。

**char *int_curr_symbol**
- 适用于当前本地化环境的国际货币符号。前三个字符包含符合 ISO 4217:2015 标准的字母国际货币符号。第四个字符（紧邻空字节之前）是用于分隔国际货币符号和货币数值的字符。

**char *currency_symbol**
- 适用于当前本地化环境的本地货币符号。

**char *mon_decimal_point**
- 用于格式化货币数值的基数字符。

**char *mon_thousands_sep**
- 在格式化的货币数值中，小数点前数字组的分隔符。

**char *mon_grouping**
- 一个字符串，其元素被视为单字节整数值，表示格式化的货币数值中每个数字组的大小。

**char *positive_sign**
- 用于表示非负格式化货币数值的字符串。

**char *negative_sign**
- 用于表示负格式化货币数值的字符串。

**char int_frac_digits**
- 在国际格式化的货币数值中要显示的小数位数（小数点后的位数）。

**char frac_digits**
- 在格式化的货币数值中要显示的小数位数（小数点后的位数）。

**char p_cs_precedes**
- 如果 `currency_symbol` 位于非负格式化货币数值之前，设置为 1。如果符号位于数值之后，设置为 0。

**char p_sep_by_space**
- 设置为一个值，表示非负格式化货币数值的 `currency_symbol`、符号字符串和数值之间的分隔。

**char n_cs_precedes**
- 如果 `currency_symbol` 位于负格式化货币数值之前，设置为 1。如果符号位于数值之后，设置为 0。

**char n_sep_by_space**
- 设置为一个值，表示负格式化货币数值的 `currency_symbol`、符号字符串和数值之间的分隔。

**char p_sign_posn**
- 设置为一个值，表示非负格式化货币数值的 `positive_sign` 的位置。

**char n_sign_posn**
- 设置为一个值，表示负格式化货币数值的 `negative_sign` 的位置。

**char int_p_cs_precedes**
- 如果 `int_curr_symbol` 分别位于非负国际格式化货币数值之前或之后，设置为 1 或 0。

**char int_n_cs_precedes**
- 如果 `int_curr_symbol` 分别位于负国际格式化货币数值之前或之后，设置为 1 或 0。

**char int_p_sep_by_space**
- 设置为一个值，表示非负国际格式化货币数值的 `int_curr_symbol`、符号字符串和数值之间的分隔。

**char int_n_sep_by_space**
- 设置为一个值，表示负国际格式化货币数值的 `int_curr_symbol`、符号字符串和数值之间的分隔。

**char int_p_sign_posn**
- 设置为一个值，表示非负国际格式化货币数值的 `positive_sign` 的位置。

**char int_n_sign_posn**
- 设置为一个值，表示负国际格式化货币数值的 `negative_sign` 的位置。

### 分组解释

`grouping` 和 `mon_grouping` 的元素按照以下规则解释：

- **{CHAR_MAX}**: 不再进行分组。
- **0**: 重复使用前一个元素来处理剩余的数字。
- **其他**: 整数值是组成当前组的数字位数。检查下一个元素以确定当前组之前下一组数字的大小。

### 空格分隔值

`p_sep_by_space`、`n_sep_by_space`、`int_p_sep_by_space` 和 `int_n_sep_by_space` 的值按照以下规则解释：

- **0**: 货币符号和数值之间没有空格分隔。
- **1**: 如果货币符号和符号字符串相邻，则它们与数值之间用空格分隔；否则，货币符号与数值之间用空格分隔。
- **2**: 如果货币符号和符号字符串相邻，则它们之间用空格分隔；否则，符号字符串与数值之间用空格分隔。

对于 `int_p_sep_by_space` 和 `int_n_sep_by_space`，使用 `int_curr_symbol` 的第四个字符代替空格。

### 符号位置值

`p_sign_posn`、`n_sign_posn`、`int_p_sign_posn` 和 `int_n_sign_posn` 的值按照以下规则解释：

- **0**: 括号包围数值和 `currency_symbol` 或 `int_curr_symbol`。
- **1**: 符号字符串位于数值和 `currency_symbol` 或 `int_curr_symbol` 之前。
- **2**: 符号字符串位于数值和 `currency_symbol` 或 `int_curr_symbol` 之后。
- **3**: 符号字符串紧邻 `currency_symbol` 或 `int_curr_symbol` 之前。
- **4**: 符号字符串紧邻 `currency_symbol` 或 `int_curr_symbol` 之后。

实现应表现得像 POSIX.1-2024 卷中没有函数调用 `localeconv()`。

`localeconv()` 函数不需要是线程安全的；但是，`localeconv()` 应避免与所有其他函数发生数据竞争。

## 返回值

`localeconv()` 函数应当返回指向填充完成对象的指针。应用程序不应修改返回值指向的结构体，也不应修改结构体内指针指向的存储区域。返回的指针和结构体内的指针可能被后续的 `localeconv()` 调用无效化，或者结构体或存储区域可能被后续调用覆盖。此外，返回的指针和结构体内的指针可能被后续的 `setlocale()` 调用（使用 LC_ALL、LC_MONETARY 或 LC_NUMERIC 类别）或更改 LC_MONETARY 或 LC_NUMERIC 类别的 `uselocale()` 调用无效化，或者结构体或存储区域可能被覆盖。如果调用线程终止，返回的指针、结构体内的指针、结构体和存储区域也可能被无效化。

## 错误

没有定义错误。

## 示例

无。

## 应用程序用法

下表说明了四个国家可能用于格式化货币数值的规则：

| 国家 | 正值格式 | 负值格式 | 国际格式 |
|---------|-----------------|-----------------|----------------------|
| 意大利 | €.1.230 | \-€.1.230 | EUR.1.230 |
| 荷兰 | € 1.234,56 | € -1.234,56 | EUR 1.234,56 |
| 挪威 | kr1.234,56 | kr1.234,56- | NOK 1.234,56 |
| 瑞士 | SFrs.1,234.56 | SFrs.1,234.56C | CHF 1,234.56 |

对于这四个国家，`localeconv()` 返回的结构体货币成员的相应值如下：

| 成员 | 意大利 | 荷兰 | 挪威 | 瑞士 |
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

## 原理

无。

## 未来方向

无。

## 另见

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

## 变更历史

### 首次发布于 Issue 4。源自 ANSI C 标准。

### Issue 6

- 在描述中添加了说明此函数不需要可重入的注释。
- 重写返回值部分以避免使用"必须"一词。
- 更新此参考页面以与 ISO/IEC 9899:1999 标准对齐。
- 纳入 ISO/IEC 9899:1999 标准、技术勘误表 1。
- 应用 IEEE Std 1003.1-2001/Cor 1-2002，项目 XSH/TC1/D6/31，删除对 `int_curr_symbol` 的引用并更新 `p_sep_by_space` 和 `n_sep_by_space` 的描述。这些更改是为了与 ISO C 标准对齐。

### Issue 7

- 应用 Austin Group 解释 1003.1-2001 #156。
- 更新 `int_curr_symbol` 和 `currency_symbol` 的定义。
- 更新应用程序用法部分的示例。
- 应用 POSIX.1-2008，技术勘误表 1，XSH/TC1-2008/0362 [75]。
- 应用 POSIX.1-2008，技术勘误表 2，XSH/TC2-2008/0200 [656]。

### Issue 8

- 应用 Austin Group 缺陷 1302，使此函数与 ISO/IEC 9899:2018 标准对齐。