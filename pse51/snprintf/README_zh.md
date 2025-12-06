# snprintf, asprintf, dprintf, fprintf, printf, sprintf — 打印格式化输出

## 概要

```c
#include <stdio.h>

[CX] int asprintf(char **restrict ptr, const char *restrict format, ...);
[CX] int dprintf(int fildes, const char *restrict format, ...);
int fprintf(FILE *restrict stream, const char *restrict format, ...);
int printf(const char *restrict format, ...);
int snprintf(char *restrict s, size_t n,
      const char *restrict format, ...);
int sprintf(char *restrict s, const char *restrict format, ...);
```

## 描述

[CX] 除了 `asprintf()`、`dprintf()` 以及传递空宽字符时 `%lc` 转换的行为外，本参考页描述的功能与 ISO C 标准一致。此处描述的要求与 ISO C 标准之间的任何其他冲突都是无意的。POSIX.1-2024 的这一卷在 `fprintf()`、`printf()`、`snprintf()` 和 `sprintf()` 功能方面遵循 ISO C 标准，但在传递空宽字符时的 `%lc` 转换方面除外。

`fprintf()` 函数应将输出放置在指定的输出流 `stream` 上。`printf()` 函数应将输出放置在标准输出流 `stdout` 上。`sprintf()` 函数应将输出后跟空字节 '\0' 放置在从 `*s` 开始的连续字节中；确保有足够的空间可用是用户的责任。

[CX] `asprintf()` 函数应等价于 `sprintf()`，不同的是输出字符串应写入动态分配的内存，该内存分配方式如同调用 `malloc()`，长度应足以容纳结果字符串，包括终止空字节。如果 `asprintf()` 调用成功，此动态分配字符串的地址应存储在 `ptr` 引用的位置。

`dprintf()` 函数应等价于 `fprintf()` 函数，不同的是 `dprintf()` 应将输出写入与 `fildes` 参数指定的文件描述符关联的文件，而不是将输出放置在流上。

`snprintf()` 函数应等价于 `sprintf()`，但增加了 `n` 参数，该参数限制写入 `s` 引用的缓冲区的字节数。如果 `n` 为零，则不应写入任何内容，且 `s` 可能为空指针。否则，超出第 `n-1` 个字节的输出字节将被丢弃而不是写入数组，并在实际写入数组的字节末尾写入一个空字节。

如果由于调用 `sprintf()` 或 `snprintf()` 导致重叠的对象之间发生复制，则结果未定义。

这些函数中的每一个都在 `format` 的控制下转换、格式化和打印其参数。应用程序应确保格式是字符字符串，如果存在任何移位状态，则以初始移位状态开始和结束。格式由零个或多个指令组成：**普通字符**（仅复制到输出流）和**转换规范**（每个都导致获取零个或多个参数）。如果格式没有足够的参数，则结果未定义。如果格式用尽而参数仍有剩余，则多余参数将被求值但除此之外被忽略。

[CX] 转换可以应用于参数列表中 `format` 之后的第 n 个参数，而不是下一个未使用的参数。在这种情况下，转换说明符字符 %（见下文）被序列 "%n$" 替换，其中 n 是范围 [1,{NL_ARGMAX}] 中的十进制整数，给出参数在参数列表中的位置。此功能提供了定义以适合特定语言的顺序选择参数的格式字符串（见示例部分）。

`format` 可以包含编号参数转换规范（即以 "%n$" 引入并可选择性包含 "*m$" 形式的字段宽度和精度）或非编号参数转换规范（即以 % 字符引入并可选择性包含 * 形式的字段宽度和精度），但不能同时包含两者。唯一的例外是 %% 可以与 "%n$" 形式混合使用。在格式字符串中混合编号和非编号参数规范的结果是未定义的。使用编号参数规范时，指定第 N 个参数要求所有前导参数（从第一个到第 (N-1) 个）都在格式字符串中指定。

在包含 "%n$" 形式转换规范的格式字符串中，参数列表中的编号参数可以根据需要从格式字符串中引用多次。

在包含 % 形式转换规范的格式字符串中，每个转换规范使用参数列表中第一个未使用的参数。

[CX] 所有形式的 `fprintf()` 函数都允许在输出字符串中插入依赖于语言的小数点字符。小数点字符在当前语言环境（类别 `LC_NUMERIC`）中定义。在 POSIX 语言环境中，或在小数点字符未定义的语言环境中，小数点字符应默认为 <period> ('.')。

每个转换规范由 '%' 字符 [CX] 或字符序列 "%n$" 引入，其后按顺序出现以下内容：

* 零个或多个**标志**（按任意顺序），它们修改转换规范的含义。
* 可选的最小**字段宽度**。如果转换后的值具有比字段宽度更少的字节，默认情况下应在左侧用 <space> 字符填充；如果给字段宽度指定左对齐标志（'-'）（如下所述），则应在右侧填充。字段宽度采用 <asterisk> ('*') 的形式，[CX] 或在以 "%n$" 引入的转换规范中采用 "*m$" 字符串（如下所述），或十进制整数的形式。
* 可选的**精度**，它给出 d、i、o、u、x 和 X 转换说明符要显示的最小数字位数；a、A、e、E、f 和 F 转换说明符的小数点后要显示的数字位数；g 和 G 转换说明符的最大有效数字位数；或者要从 s [XSI] 和 S 转换说明符中的字符串打印的最大字节数。精度采用 <period> ('.') 后跟 <asterisk> ('*') 的形式，[CX] 或在以 "%n$" 引入的转换规范中采用 "*m$" 字符串（如下所述），或可选的十进制数字字符串的形式，其中空数字字符串被视为零。如果精度与任何其他转换说明符一起出现，则行为未定义。
* 可选的**长度修饰符**，指定参数的大小。
* **转换说明符**字符，指示要应用的转换类型。

字段宽度、精度或两者都可以用 <asterisk> ('*') 表示。在这种情况下，`int` 类型的参数提供字段宽度或精度。应用程序应确保指定字段宽度、精度或两者的参数按此顺序出现在要转换的参数（如果有）之前。负字段宽度被视为 '-' 标志后跟正字段宽度。负精度被视为省略精度。[CX] 在包含以 "%n$" 引入的转换规范的格式字符串中，除了用十进制数字字符串指示外，字段宽度可以用序列 "*m$" 表示，精度可以用序列 ".*m$" 表示，其中 m 是范围 [1,{NL_ARGMAX}] 中的十进制整数，给出包含字段宽度或精度的整数参数在参数列表中的位置（在 `format` 参数之后），例如：

```c
printf("%1$d:%2$.*3$d:%4$.*3$d\n", hour, min, precision, sec);
```

### 标志字符

标志字符及其含义是：

**'** (撇号)
[CX] 十进制转换（%i、%d、%u、%f、%F、%g 或 %G）结果的整数部分应使用千位分组字符格式化。对于其他转换，行为未定义。使用非货币分组字符。

**-**
转换的结果应在字段内左对齐。如果未指定此标志，则转换右对齐。

**+**
有符号转换的结果应始终以符号（'+' 或 '-'）开始。如果未指定此标志，则仅当转换负值时转换才以符号开始。

**<space>**
如果有符号转换的第一个字符不是符号，或者有符号转换的结果没有字符，则应在结果前缀 <space>。这意味着如果 <space> 和 '+' 标志都出现，则应忽略 <space> 标志。

**#**
指定值要转换为替代形式。对于 o 转换，它应增加精度，当且仅当必要时，以强制结果的第一个数字为零（如果值和精度都为 0，则打印单个 0）。对于 x 或 X 转换说明符，非零结果应前缀 0x（或 0X）。对于 a、A、e、E、f、F、g 和 G 转换说明符，结果应始终包含小数点字符，即使小数点字符后没有数字。没有此标志，仅当小数点字符后跟数字时，这些转换的结果中才会出现小数点字符。对于 g 和 G 转换说明符，尾随零**不应**从结果中删除，就像它们通常被删除一样。对于其他转换说明符，行为未定义。

**0**
对于 d、i、o、u、x、X、a、A、e、E、f、F、g 和 G 转换说明符，前导零（在任何符号或基数指示之后）用于填充到字段宽度，而不是执行空格填充，转换无穷大或 NaN 时除外。如果 '0' 和 '-' 标志都出现，则忽略 '0' 标志。对于 d、i、o、u、x 和 X 转换说明符，如果指定了精度，则应忽略 '0' 标志。[CX] 如果 '0' 和 <apostrophe> 标志都出现，则分组字符在零填充之前插入。对于其他转换，行为未定义。

### 长度修饰符

长度修饰符及其含义是：

**hh**
指定后续的 d、i、o、u、x 或 X 转换说明符应用于 `signed char` 或 `unsigned char` 参数（参数将根据整数提升进行提升，但其值应在打印前转换为 `signed char` 或 `unsigned char`）；或者后续的 n 转换说明符应用于指向 `signed char` 参数的指针。

**h**
指定后续的 d、i、o、u、x 或 X 转换说明符应用于 `short` 或 `unsigned short` 参数（参数将根据整数提升进行提升，但其值应在打印前转换为 `short` 或 `unsigned short`）；或者后续的 n 转换说明符应用于指向 `short` 参数的指针。

**l** (ell)
指定后续的 d、i、o、u、x 或 X 转换说明符应用于 `long` 或 `unsigned long` 参数；后续的 n 转换说明符应用于指向 `long` 参数的指针；后续的 c 转换说明符应用于 `wint_t` 参数；后续的 s 转换说明符应用于指向 `wchar_t` 参数的指针；或者对后续的 a、A、e、E、f、F、g 或 G 转换说明符没有影响。

**ll** (ell-ell)
指定后续的 d、i、o、u、x 或 X 转换说明符应用于 `long long` 或 `unsigned long long` 参数；或者后续的 n 转换说明符应用于指向 `long long` 参数的指针。

**j**
指定后续的 d、i、o、u、x 或 X 转换说明符应用于 `intmax_t` 或 `uintmax_t` 参数；或者后续的 n 转换说明符应用于指向 `intmax_t` 参数的指针。

**z**
指定后续的 d、i、o、u、x 或 X 转换说明符应用于 `size_t` 或相应的有符号整数类型参数；或者后续的 n 转换说明符应用于指向对应于 `size_t` 参数的有符号整数类型的指针。

**t**
指定后续的 d、i、o、u、x 或 X 转换说明符应用于 `ptrdiff_t` 或相应的 `unsigned` 类型参数；或者后续的 n 转换说明符应用于指向 `ptrdiff_t` 参数的指针。

**L**
指定后续的 a、A、e、E、f、F、g 或 G 转换说明符应用于 `long double` 参数。

如果长度修饰符与除上述指定之外的任何转换说明符一起出现，则行为未定义。

### 转换说明符

转换说明符及其含义是：

**d, i**
`int` 参数应转换为 "[-]dddd" 样式的有符号十进制数。精度指定要显示的最小数字位数；如果要转换的值可以用更少的位数表示，则应用前导零扩展。默认精度为 1。用显式精度零转换零的结果应为无字符。

**o**
`unsigned` 参数应转换为 "dddd" 样式的无符号八进制数。精度指定要显示的最小数字位数；如果要转换的值可以用更少的位数表示，则应用前导零扩展。默认精度为 1。用显式精度零转换零的结果应为无字符。

**u**
`unsigned` 参数应转换为 "dddd" 样式的无符号十进制数。精度指定要显示的最小数字位数；如果要转换的值可以用更少的位数表示，则应用前导零扩展。默认精度为 1。用显式精度零转换零的结果应为无字符。

**x**
`unsigned` 参数应转换为 "dddd" 样式的无符号十六进制数；使用字母 "abcdef"。精度指定要显示的最小数字位数；如果要转换的值可以用更少的位数表示，则应用前导零扩展。默认精度为 1。用显式精度零转换零的结果应为无字符。

**X**
等价于 x 转换说明符，但使用字母 "ABCDEF" 而不是 "abcdef"。

**f, F**
`double` 参数应转换为 "[-]ddd.ddd" 样式的十进制记数法，其中小数点字符后的数字位数等于精度规范。如果缺少精度，则应取为 6；如果精度显式为零且没有 '#' 标志，则不应出现小数点字符。如果出现小数点字符，则它前面至少出现一个数字。低位数字应以实现定义的方式四舍五入。

表示无穷大的 `double` 参数应转换为 "[-]inf" 或 "[-]infinity" 样式之一；使用哪种样式是实现定义的。表示 NaN 的 `double` 参数应转换为 "[-]nan(n-char-sequence)" 或 "[-]nan" 样式之一；使用哪种样式以及任何 n-char-sequence 的含义是实现定义的。F 转换说明符产生 "INF"、"INFINITY" 或 "NAN"，而不是 "inf"、"infinity" 或 "nan"。

**e, E**
`double` 参数应转换为 "[-]d.ddd_e±dd" 样式，其中小数点字符前有一个数字（如果参数非零则非零），其后的数字位数等于精度；如果缺少精度，则应取为 6；如果精度为零且没有 '#' 标志，则不应出现小数点字符。低位数字应以实现定义的方式四舍五入。E 转换说明符应产生用 'E' 而不是 'e' 引入指数的数字。指数应始终包含至少两位数字。如果值为零，则指数应为零。

表示无穷大或 NaN 的 `double` 参数应以 f 或 F 转换说明符的样式转换。

**g, G**
表示浮点数的 `double` 参数应根据转换的值和精度以 f 或 e 样式转换（或者在 G 转换说明符的情况下以 F 或 E 样式转换）。令 P 等于精度（如果非零），省略精度时为 6，或者精度为零时为 1。那么，如果使用 E 样式的转换的指数为 X：

* 如果 P>X>=-4，则转换应使用 f（或 F）样式和精度 P-(X+1)。
* 否则，转换应使用 e（或 E）样式和精度 P-1。

最后，除非使用 '#' 标志，否则应从结果的小数部分中删除任何尾随零，如果没有剩余的小数部分，则应删除小数点字符。

表示无穷大或 NaN 的 `double` 参数应以 f 或 F 转换说明符的样式转换。

**a, A**
表示浮点数的 `double` 参数应转换为 "[-]0x_h._hhhh_p±d" 样式，其中小数点字符前有一个十六进制数字（如果参数是归一化浮点数则非零，否则未指定），其后的十六进制数字位数等于精度；如果缺少精度且 FLT_RADIX 是 2 的幂，则精度应足以精确表示值；如果缺少精度且 FLT_RADIX 不是 2 的幂，则精度应足以区分 `double` 类型的值，但可以省略尾随零；如果精度为零且未指定 '#' 标志，则不应出现小数点字符。a 转换使用字母 "abcdef"，A 转换使用字母 "ABCDEF"。A 转换说明符产生用 'X' 和 'P' 而不是 'x' 和 'p' 的数字。指数应始终包含至少一位数字，并且仅包含表示 2 的十进制指数所需的更多数字。如果值为零，则指数应为零。

表示无穷大或 NaN 的 `double` 参数应以 f 或 F 转换说明符的样式转换。

**c**
`int` 参数应转换为 `unsigned char`，结果字节应被写入。

如果存在 l (ell) 限定符，[CX] `wint_t` 参数应转换为多字节序列，如同通过调用 `wcrtomb()` 并使用指向至少 MB_CUR_MAX 字节的存储指针、转换为 `wchar_t` 的 `wint_t` 参数以及初始移位状态，并写入结果字节。

**s**
参数应是指向 `char` 数组的指针。应从数组中写入字节，直到（但不包括）任何终止空字节。如果指定了精度，则写入的字节数不应超过该数量。如果未指定精度或精度大于数组大小，应用程序应确保数组包含空字节。

如果存在 l (ell) 限定符，参数应是指向 `wchar_t` 类型数组的指针。应将数组中的宽字符转换为字符（每个都如同通过调用 `wcrtomb()` 函数，转换状态由在第一个宽字符转换前初始化为零的 `mbstate_t` 对象描述），直到并包括终止空宽字符。应写入结果字符，直到（但不包括）终止空字符（字节）。如果未指定精度，应用程序应确保数组包含空宽字符。如果指定了精度，则写入的字符（字节）数不应超过该数量（包括任何移位序列），并且如果为了等于精度给定的字符序列长度，函数需要访问数组末尾之后的宽字符，则数组应包含空宽字符。在任何情况下都不应写入部分字符。

**p**
参数应是指向 `void` 的指针。指针的值以实现定义的方式转换为可打印字符序列。

**n**
参数应是指向整数的指针，其中写入通过调用 `fprintf()` 函数之一到目前为止写入输出的字节数。不转换任何参数。

**C**
[XSI] 等价于 lc。

**S**
[XSI] 等价于 ls。

**%**
写入 '%' 字符；不转换任何参数。应用程序应确保完整的转换规范是 %%。

如果转换规范与上述形式之一不匹配，则行为未定义。如果任何参数不是相应转换规范的正确类型，则行为未定义。

在任何情况下，不存在或小的字段宽度都不会导致字段截断；如果转换的结果比字段宽度宽，则应扩展字段以包含转换结果。`fprintf()` 和 `printf()` 生成的字符被打印，如同调用了 `fputc()`。

对于 a 和 A 转换说明符，如果 FLT_RADIX 是 2 的幂，则值应正确舍入为具有给定精度的十六进制浮点数。

对于 a 和 A 转换，如果 FLT_RADIX 不是 2 的幂且结果在给定精度下不能精确表示，则结果应该是给定精度的十六进制浮点样式中的两个相邻数字之一，额外要求误差对于当前舍入方向应有正确符号。

对于 e、E、f、F、g 和 G 转换说明符，如果有效十进制数字的数量最多为 DECIMAL_DIG，则结果应正确舍入。如果有效十进制数字的数量超过 DECIMAL_DIG 但源值可以用 DECIMAL_DIG 位数字精确表示，则结果应该是带尾随零的精确表示。否则，源值由两个相邻的十进制字符串 L < U 限定，两者都具有 DECIMAL_DIG 个有效数字；结果十进制字符串 D 的值应满足 L <= D <= U，额外要求误差对于当前舍入方向应有正确符号。

[CX] 文件的最后数据修改和最后文件状态更改时间戳应标记为更新：

1. 在调用 `fprintf()` 或 `printf()` 成功执行与在同一流上成功完成调用 `fflush()` 或 `fclose()` 或调用 `exit()` 或 `abort()` 之间
2. 在 `dprintf()` 调用成功完成时

## 返回值

成功完成后，[CX] `dprintf()`、`fprintf()` 和 `printf()` 函数应返回传输的字节数。

[CX] 成功完成后，`asprintf()` 函数应返回写入存储在 `ptr` 引用位置的已分配字符串的字节数，不包括终止空字节。

成功完成后，`sprintf()` 函数应返回写入 `s` 的字节数，不包括终止空字节。

成功完成后，`snprintf()` 函数应返回如果 `n` 足够大时将写入 `s` 的字节数，不包括终止空字节。

如果遇到错误，这些函数应返回负值 [CX] 并设置 `errno` 以指示错误。对于 `asprintf()`，如果内存分配不可能，或者发生其他错误，函数应返回负值，`ptr` 引用的位置内容未定义，但不应引用已分配的内存。

如果在调用 `snprintf()` 时 `n` 的值为零，则不应写入任何内容，应返回如果 `n` 足够大时将要写入的字节数（不包括终止空字节），并且 `s` 可能为空指针。

## 错误

有关 [CX] `dprintf()`、`fprintf()` 和 `printf()` 失败和可能失败的条件，请参考 `fputc()` 或 `fputwc()`。

此外，所有形式的 `fprintf()` 在以下情况下将失败：

**[EILSEQ]**
[CX] 检测到不对应于有效字符的宽字符代码。

**[EOVERFLOW]**
[CX] 要返回的值大于 {INT_MAX}。

[CX] `asprintf()` 函数在以下情况下将失败：

**[ENOMEM]**
存储空间不足。

`dprintf()` 函数在以下情况下可能失败：

**[EBADF]**
`fildes` 参数不是有效的文件描述符。

[CX] `dprintf()`、`fprintf()` 和 `printf()` 函数在以下情况下可能失败：

**[ENOMEM]**
[CX] 存储空间不足。

## 示例

### 打印语言无关的日期和时间

以下语句可用于使用语言无关格式打印日期和时间：

```c
printf(format, weekday, month, day, hour, min);
```

对于美国用法，`format` 可以是指向以下字符串的指针：

```c
"%s, %s %d, %d:%.2d\n"
```

此示例将产生以下消息：

```
Sunday, July 3, 10:02
```

对于德国用法，`format` 可以是指向以下字符串的指针：

```c
"%1$s, %3$d. %2$s, %4$d:%5$.2d\n"
```

此 `format` 定义将产生以下消息：

```
Sonntag, 3. Juli, 10:02
```

### 打印文件信息

以下示例打印目录中特定文件的类型、权限和链接数信息。

```c
#include <stdio.h>
#include <sys/types.h>
#include <pwd.h>
#include <grp.h>

char *strperm (mode_t);
...
struct stat statbuf;
struct passwd *pwd;
struct group *grp;
...
printf("%10.10s", strperm (statbuf.st_mode));
printf("%4d", statbuf.st_nlink);

if ((pwd = getpwuid(statbuf.st_uid)) != NULL)
    printf(" %-8.8s", pwd->pw_name);
else
    printf(" %-8ld", (long) statbuf.st_uid);

if ((grp = getgrgid(statbuf.st_gid)) != NULL)
    printf(" %-8.8s", grp->gr_name);
else
    printf(" %-8ld", (long) statbuf.st_gid);

printf("%9jd", (intmax_t) statbuf.st_size);
```

### 打印本地化日期字符串

以下示例获取本地化日期字符串：

```c
#include <stdio.h>
#include <time.h>
#include <langinfo.h>
...
struct dirent *dp;
struct tm *tm;
char datestring[256];
...
strftime(datestring, sizeof(datestring), nl_langinfo (D_T_FMT), tm);

printf(" %s %s\n", datestring, dp->d_name);
```

### 打印错误信息

以下示例使用 `fprintf()` 将错误信息写入标准错误：

```c
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <errno.h>

#define LOCKFILE "/etc/ptmp"
#define PASSWDFILE "/etc/passwd"
...
int pfd;
...
if ((pfd = open(LOCKFILE, O_WRONLY | O_CREAT | O_EXCL,
    S_IRUSR | S_IWUSR | S_IRGRP | S_IROTH)) == -1)
{
    fprintf(stderr, "Cannot open /etc/ptmp. Try again later.\n");
    exit(1);
}
...
if (link(LOCKFILE,PASSWDFILE) == -1) {
    fprintf(stderr, "Link error: %s\n", strerror(errno));
    exit(1);
}
```

### 打印用法信息

以下示例检查程序是否有必要的参数，如果没有预期的参数数量，则使用 `fprintf()` 打印用法信息：

```c
#include <stdio.h>
#include <stdlib.h>
...
char *Options = "hdbtl";
...
if (argc < 2) {
    fprintf(stderr, "Usage: %s -%s <file\n", argv[0], Options); exit(1);
}
```

### 格式化十进制字符串

以下示例在 `stdout` 上打印键和数据对。注意格式字符串中 <asterisk> ('*') 的使用；这确保根据请求的元素数量为元素提供正确的小数位数：

```c
#include <stdio.h>
...
long i;
char *keystr;
int elementlen, len;
...
while (len < elementlen) {
...
    printf("%s Element%0*ld\n", keystr, elementlen, i);
...
}
```

### 创建路径名

以下示例使用先前返回用户密码数据库条目的 `getpwnam()` 函数的信息创建路径名：

```c
#include <stdint.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/types.h>
#include <unistd.h>
...
char *pathname;
struct passwd *pw;
size_t len;
...
// pid_t 所需的位数是位数乘以
// log2(10) = 约 10/33
len = strlen(pw->pw_dir) + 1 + 1+(sizeof(pid_t)*80+32)/33 +
    sizeof ".out";
pathname = malloc(len);
if (pathname != NULL)
{
    snprintf(pathname, len, "%s/%jd.out", pw->pw_dir,
        (intmax_t)getpid());
    ...
}
```

### 报告事件

以下示例循环直到事件超时。`pause()` 函数除非接收到信号，否则永远等待。由于 `pause()` 的可能返回值，`fprintf()` 语句永远不应该出现：

```c
#include <stdio.h>
#include <unistd.h>
#include <string.h>
#include <errno.h>
...
while (!event_complete) {
...
    if (pause() != -1 || errno != EINTR)
        fprintf(stderr, "pause: unknown error: %s\n", strerror(errno));
}
```

### 打印货币信息

以下示例使用 `strfmon()` 转换数字并将其存储为名为 `convbuf` 的格式化货币字符串。如果打印第一个数字，程序打印格式和描述；否则，它只打印数字：

```c
#include <monetary.h>
#include <stdio.h>
...
struct tblfmt {
    char *format;
    char *description;
};

struct tblfmt table[] = {
    { "%n", "default formatting" },
    { "%11n", "right align within an 11 character field" },
    { "%#5n", "aligned columns for values up to 99999" },
    { "%=*#5n", "specify a fill character" },
    { "%=0#5n", "fill characters do not use grouping" },
    { "%^#5n", "disable the grouping separator" },
    { "%^#5.0n", "round off to whole units" },
    { "%^#5.4n", "increase the precision" },
    { "%(#5n", "use an alternative pos/neg style" },
    { "%!(#5n", "disable the currency symbol" },
};
...
float input[3];
int i, j;
char convbuf[100];
...
strfmon(convbuf, sizeof(convbuf), table[i].format, input[j]);

if (j == 0) {
    printf("%s  %s      %s\n", table[i].format,
        convbuf, table[i].description);
}
else {
    printf("    %s\n", convbuf);
}
```

### 打印宽字符

以下示例打印一系列宽字符。假设 "L`@`" 扩展为三个字节：

```c
wchar_t wz [3] = L"@@";       // 零终止
wchar_t wn [3] = L"@@";      // 非终止

fprintf (stdout,"%ls", wz);   // 输出 6 字节
fprintf (stdout,"%ls", wn);   // 未定义，因为 wn 没有终止符
fprintf (stdout,"%4ls", wz);  // 输出 3 字节
fprintf (stdout,"%4ls", wn);  // 输出 3 字节；不需要终止符
fprintf (stdout,"%9ls", wz);  // 输出 6 字节
fprintf (stdout,"%9ls", wn);  // 输出 9 字节；不需要终止符
fprintf (stdout,"%10ls", wz); // 输出 6 字节
fprintf (stdout,"%10ls", wn); // 未定义，因为 wn 没有终止符
```

在示例的最后一行中，处理三个字符后，已输出九个字节。然后必须检查第四个字符以确定它是转换为一个字节还是更多字节。如果它转换为多个字节，则输出只有九个字节。由于数组中没有第四个字符，行为未定义。

## 应用程序用法

如果调用 `fprintf()` 的应用程序具有任何 `wint_t` 或 `wchar_t` 类型的对象，它还必须包含 `<wchar.h>` 头文件以定义这些对象。

成功调用 `asprintf()` 分配的空间应随后通过调用 `free()` 释放。

## 基本原理

如果实现检测到格式没有足够的参数，建议函数应失败并报告 [EINVAL] 错误。

为 %lc 转换指定的行为与 ISO C 标准中的规范略有不同，打印空宽字符产生空字节而不是 0 字节输出，如同对 ISO C 标准指示应用 %ls 说明符到第一个元素为空宽字符的 `wchar_t` 数组的严格读法所要求的那样。要求每个可能的宽字符（包括空字符）都有多字节输出符合历史实践，并与 `fprintf()` 中的 %c 以及 `fwprintf()` 中的 %c 和 %lc 提供一致性。预期 ISO C 标准的未来版本将更改以匹配此处指定的行为。

## 未来方向

无。

## 另请参见

* [2.5 标准 I/O 流](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_05)
* [`fputc()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fputc.html)
* [`fscanf()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fscanf.html)
* [`setlocale()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/setlocale.html)
* [`strfmon()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/strfmon.html)
* [`strlcat()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/strlcat.html)
* [`wcrtomb()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/wcrtomb.html)
* [`wcslcat()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/wcslcat.html)

XBD [7. 语言环境](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/V1_chap07.html#tag_07), [`<inttypes.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/inttypes.h.html), [`<stdio.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdio.h.html), [`<wchar.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/wchar.h.html)

## 更改历史

### 首次在 Issue 1 中发布。源自 SVID 的 Issue 1。

### Issue 5

与 ISO/IEC 9899:1990/Amendment 1:1995 (E) 对齐。具体来说，l (ell) 限定符现在可以与 c 和 s 转换说明符一起使用。

`snprintf()` 函数在 Issue 5 中是新的。

### Issue 6

超出 ISO C 标准的扩展被标记。

规范文本更新，避免对应用程序要求使用术语"必须"。

为与 ISO/IEC 9899:1999 标准对齐进行以下更改：

* `fprintf()`、`printf()`、`snprintf()` 和 `sprintf()` 的原型更新，并从 `snprintf()` 中移除 XSI 阴影。
* `snprintf()` 的描述与 ISO C 标准对齐。注意，这取代了 Open Group Base Resolution bwg98-006 中的 `snprintf()` 描述，该描述改变了 Issue 5 的行为。
* 描述更新。

描述更新，一致使用术语"转换说明符"和"转换规范"。

合并 ISO/IEC 9899:1999 标准，技术勘误表 1。

添加打印宽字符的示例。

### Issue 7

应用 Austin Group Interpretation 1003.1-2001 #161，更新 0 标志的描述。

应用 Austin Group Interpretation 1003.1-2001 #170。

应用 ISO/IEC 9899:1999 标准，技术勘误表 2 #68 (SD5-XSH-ERN-70)，修订 g 和 G 的描述。

应用 SD5-XSH-ERN-174。

从 The Open Group Technical Standard, 2006, Extended API Set Part 1 添加 `dprintf()` 函数。

与 %n$ 形式转换规范和 <apostrophe> 标志相关的功能从 XSI 选项移至 Base。

进行与支持细粒度时间戳相关的更改。

应用 POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0163 [302], XSH/TC1-2008/0164 [316], XSH/TC1-2008/0165 [316], XSH/TC1-2008/0166 [451,291], 和 XSH/TC1-2008/0167 [14]。

应用 POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0126 [894], XSH/TC2-2008/0127 [557], 和 XSH/TC2-2008/0128 [936]。

### Issue 8

应用 Austin Group Defect 986，将 `strlcat()` 和 `wcslcat()` 添加到另请参见部分。

应用 Austin Group Defect 1020，阐明 `snprintf()` 参数 `n` 限制写入 `s` 的字节数；它不一定与 `s` 的大小相同。

应用 Austin Group Defect 1021，将返回值部分中的"输出错误"更改为"错误"。

应用 Austin Group Defect 1137，阐明在转换规范中使用 "%n$" 和 "*m$"。

应用 Austin Group Defect 1205，更改 % 转换说明符的描述。

应用 Austin Group Defect 1219，删除 `snprintf()` 特定的 [EOVERFLOW] 错误。

应用 Austin Group Defect 1496，添加 `asprintf()` 函数。

应用 Austin Group Defect 1562，阐明确保格式是字符字符串（如果存在任何移位状态，则以初始移位状态开始和结束）是应用程序的责任。

应用 Austin Group Defect 1647，更改 c 转换说明符的描述，并更新本卷 POSIX.1-2024 遵循 ISO C 标准的语句，以便它排除传递空宽字符时的 %lc 转换。

---
