# printf

## 概要

```c
#include <stdio.h>

int asprintf(char **restrict ptr, const char *restrict format, ...);
int dprintf(int fildes, const char *restrict format, ...);
int fprintf(FILE *restrict stream, const char *restrict format, ...);
int printf(const char *restrict format, ...);
int snprintf(char *restrict s, size_t n,
             const char *restrict format, ...);
int sprintf(char *restrict s, const char *restrict format, ...);
```

## 描述

除了 asprintf()、dprintf() 以及传入空宽字符时 %lc 转换的行为外，本参考页描述的功能与 ISO C 标准一致。此处描述的要求与 ISO C 标准之间的任何其他冲突都是无意的。POSIX.1-2024 的这一卷在涉及传入空宽字符时的 %lc 转换方面，遵从 ISO C 标准处理所有 fprintf()、printf()、snprintf() 和 sprintf() 功能。

fprintf() 函数应将输出放置在指定的输出流上。printf() 函数应将输出放置在标准输出流 stdout 上。sprintf() 函数应将输出后跟空字节 '\0' 放置在从 *s 开始的连续字节中；用户有责任确保有足够的空间可用。

asprintf() 函数应等同于 sprintf()，不同之处在于输出字符串应写入动态分配的内存，该内存在长度上足以容纳生成的字符串（包括终止空字节），分配方式如同调用 malloc()。如果对 asprintf() 的调用成功，则此动态分配字符串的地址应存储在 ptr 引用的位置。

dprintf() 函数应等同于 fprintf() 函数，不同之处在于 dprintf() 应将输出写入与 fildes 参数指定的文件描述符关联的文件，而不是将输出放置在流上。

snprintf() 函数应等同于 sprintf()，增加了 n 参数，该参数限制写入 s 引用的缓冲区的字节数。如果 n 为零，则不应写入任何内容，s 可能为空指针。否则，超过第 n-1 个的输出字节将被丢弃而不是写入数组，并且在实际写入数组的字节末尾写入一个空字节。

如果由于调用 sprintf() 或 snprintf() 导致在重叠的对象之间进行复制，则结果未定义。

这些函数中的每个都在格式的控制下转换、格式化并打印其参数。应用程序应确保格式是一个字符串，如果存在初始转换状态，则在该状态下开始和结束。格式由零个或多个指令组成：普通字符（简单地复制到输出流）和转换规范（每个都将导致获取零个或多个参数）。如果格式没有足够的参数，则结果未定义。如果格式用尽而参数仍有剩余，则多余的参数将被求值，但除此之外将被忽略。

转换可以应用于参数列表中格式之后的第 n 个参数，而不是下一个未使用的参数。在这种情况下，转换说明符字符 %（见下文）被序列 "%n$" 替换，其中 n 是范围 [1,{NL_ARGMAX}] 内的十进制整数，给出参数在参数列表中的位置。此功能提供了定义以适合特定语言的顺序选择参数的格式字符串的功能（请参阅示例部分）。

格式可以包含编号参数转换规范（即由 "%n$" 引入并可选包含字段宽度和精度的 "*m$" 形式），或未编号参数转换规范（即由 % 字符引入并可选包含字段宽度和精度的 * 形式），但不能两者兼有。唯一的例外是 %% 可以与 "%n$" 形式混合使用。在格式字符串中混合编号和未编号参数规范的结果未定义。使用编号参数规范时，指定第 N 个参数要求所有前导参数（从第一个到第 (N-1) 个）都在格式字符串中指定。

在包含 "%n$" 形式转换规范的格式字符串中，参数列表中的编号参数可以从格式字符串中根据需要引用多次。

在包含 % 形式转换规范的格式字符串中，每个转换规范使用参数列表中的第一个未使用参数。

所有形式的 fprintf() 函数都允许在输出字符串中插入与语言相关的基数字符。基数字符在当前语言环境（类别 LC_NUMERIC）中定义。在 POSIX 语言环境或未定义基数字符的语言环境中，基数字符应默认为句点 ('.')。

每个转换规范由 '%' 字符或字符序列 "%n$" 引入，其后依次出现以下内容：

1. 零个或多个标志（以任意顺序），它们修改转换规范的含义。
2. 可选的最小字段宽度。如果转换后的值具有比字段宽度更少的字节，则默认应在左侧用空格字符填充；如果给出了左调整标志 ('-）（如下所述），则应在右侧填充。字段宽度采用星号 ('*') 的形式，或在由 "%n$" 引入的转换规范中采用下面描述的 "*m$" 字符串，或十进制整数的形式。
3. 可选的精度，它给出 d、i、o、u、x 和 X 转换说明符要出现的最小数字位数；a、A、e、E、f 和 F 转换说明符要在基数字符后出现的数字位数；g 和 G 转换说明符的最大有效数字位数；或要从 s 和 S 转换说明符打印的字符串的最大字节数。精度采用句点 ('.') 后跟星号 ('*') 的形式，或在由 "%n$" 引入的转换规范中采用下面描述的 "*m$" 字符串，或可选的十进制数字字符串的形式，其中空数字字符串被视为零。如果精度与任何其他转换说明符一起出现，则行为未定义。
4. 可选的长度修饰符，指定参数的大小。
5. 转换说明符字符，指示要应用的转换类型。

字段宽度、精度或两者都可以用星号 ('*') 表示。在这种情况下，int 类型的参数提供字段宽度或精度。应用程序应确保指定字段宽度、精度或两者的参数按该顺序出现在要转换的参数（如果有）之前。负的字段宽度被视为 '-' 标志后跟正的字段宽度。负的精度被视为精度被省略。在包含由 "%n$" 引入的转换规范的格式字符串中，除了用十进制数字字符串表示外，字段宽度还可以用序列 "*m$" 表示，精度用序列 ".*m$" 表示，其中 m 是范围 [1,{NL_ARGMAX}] 内的十进制整数，给出包含字段宽度或精度的整数参数在参数列表中的位置（在格式参数之后），例如：

```c
printf("%1$d:%2$.*3$d:%4$.*3$d\n", hour, min, precision, sec);
```

## 标志字符

标志字符及其含义是：

**'**（撇号） - 十进制转换（%i、%d、%u、%f、%F、%g 或 %G）结果的整数部分应使用千位分组字符格式化。对于其他转换，行为未定义。使用非货币分组字符。

**-** - 转换结果应在字段内左对齐。如果未指定此标志，则转换右对齐。

**+** - 有符号转换的结果应始终以符号（'+' 或 '-'）开头。如果未指定此标志，则仅当转换负值时才以符号开头。

**（空格）** - 如果有符号转换的第一个字符不是符号，或者有符号转换不产生任何字符，则应在结果前加空格。这意味着如果空格和 '+' 标志同时出现，则忽略空格标志。

**#** - 指定值要转换为替代形式。对于 o 转换，它应增加精度，当且仅当需要时，以强制结果的第一个数字为零（如果值和精度都为 0，则打印单个 0）。对于 x 或 X 转换说明符，非零结果应前缀 0x（或 0X）。对于 a、A、e、E、f、F、g 和 G 转换说明符，结果应始终包含基数字符，即使基数字符后没有数字。没有此标志时，基数字符仅当后有数字时才出现在这些转换的结果中。对于 g 和 G 转换说明符，结果末尾的零不应像通常那样被删除。对于其他转换说明符，行为未定义。

**0** - 对于 d、i、o、u、x、X、a、A、e、E、f、F、g 和 G 转换说明符，使用前导零（在任何符号或基数指示之后）填充到字段宽度，而不是执行空格填充，除非转换无穷大或 NaN。如果 '0' 和 '-' 标志同时出现，则忽略 '0' 标志。对于 d、i、o、u、x 和 X 转换说明符，如果指定了精度，则忽略 '0' 标志。如果 '0' 和撇号标志同时出现，则在零填充之前插入分组字符。对于其他转换，行为未定义。

## 长度修饰符

长度修饰符及其含义是：

**hh** - 指定后续的 d、i、o、u、x 或 X 转换说明符应用于 signed char 或 unsigned char 参数（参数将根据整数提升被提升，但其值应在打印前转换为 signed char 或 unsigned char）；或指定后续的 n 转换说明符应用于指向 signed char 参数的指针。

**h** - 指定后续的 d、i、o、u、x 或 X 转换说明符应用于 short 或 unsigned short 参数（参数将根据整数提升被提升，但其值应在打印前转换为 short 或 unsigned short）；或指定后续的 n 转换说明符应用于指向 short 参数的指针。

**l**（小写字母 l）- 指定后续的 d、i、o、u、x 或 X 转换说明符应用于 long 或 unsigned long 参数；指定后续的 n 转换说明符应用于指向 long 参数的指针；指定后续的 c 转换说明符应用于 wint_t 参数；指定后续的 s 转换说明符应用于指向 wchar_t 参数的指针；或对后续的 a、A、e、E、f、F、g 或 G 转换说明符没有影响。

**ll**（小写字母 l-l）- 指定后续的 d、i、o、u、x 或 X 转换说明符应用于 long long 或 unsigned long long 参数；或指定后续的 n 转换说明符应用于指向 long long 参数的指针。

**j** - 指定后续的 d、i、o、u、x 或 X 转换说明符应用于 intmax_t 或 uintmax_t 参数；或指定后续的 n 转换说明符应用于指向 intmax_t 参数的指针。

**z** - 指定后续的 d、i、o、u、x 或 X 转换说明符应用于 size_t 或相应的有符号整数类型参数；或指定后续的 n 转换说明符应用于指向与 size_t 参数对应的有符号整数类型的指针。

**t** - 指定后续的 d、i、o、u、x 或 X 转换说明符应用于 ptrdiff_t 或相应的无符号类型参数；或指定后续的 n 转换说明符应用于指向 ptrdiff_t 参数的指针。

**L** - 指定后续的 a、A、e、E、f、F、g 或 G 转换说明符应用于 long double 参数。

如果长度修饰符与除上述指定外的任��转换说明符一起出现，则行为未定义。

## 转换说明符

转换说明符及其含义是：

**d, i** - int 参数应转换为 "[-]dddd" 形式的有符号十进制数。精度指定要出现的最小数字位数；如果要转换的值可以用更少的位数表示，则应用前导零扩展。默认精度为 1。用显式精度零转换零的结果应为无字符。

**o** - 无符号参数应转换为 "dddd" 形式的无符号八进制数。精度指定要出现的最小数字位数；如果要转换的值可以用更少的位数表示，则应用前导零扩展。默认精度为 1。用显式精度零转换零的结果应为无字符。

**u** - 无符号参数应转换为 "dddd" 形式的无符号十进制数。精度指定要出现的最小数字位数；如果要转换的值可以用更少的位数表示，则应用前导零扩展。默认精度为 1。用显式精度零转换零的结果应为无字符。

**x** - 无符号参数应转换为 "dddd" 形式的无符号十六进制数；使用字母 "abcdef"。精度指定要出现的最小数字位数；如果要转换的值可以用更少的位数表示，则应用前导零扩展。默认精度为 1。用显式精度零转换零的结果应为无字符。

**X** - 等同于 x 转换说明符，但使用字母 "ABCDEF" 而不是 "abcdef"。

**f, F** - double 参数应转换为 "[-]ddd.ddd" 形式的十进制表示法，其中基数字符后的数字位数等于精度规范。如果精度缺失，则视为 6；如果精度显式为零且没有 '#' 标志，则不应出现基数字符。如果出现基数字符，则它前面至少出现一个数字。低位数字应以实现定义的方式四舍五入。

表示无穷大的 double 参数应转换为 "[-]inf" 或 "[-]infinity" 之一的形式；使用哪种形式是实现定义的。表示 NaN 的 double 参数应转换为 "[-]nan(n-char-sequence)" 或 "[-]nan" 之一的形式；使用哪种形式以及任何 n-char-sequence 的含义是实现定义的。F 转换说明符产生 "INF"、"INFINITY" 或 "NAN" 而不是 "inf"、"infinity" 或 "nan"。

**e, E** - double 参数应转换为 "[-]d.ddd_e±dd" 形式，其中基数字符前有一个数字（如果参数非零，则该数字非零），其后的数字位数等于精度；如果精度缺失，则视为 6；如果精度为零且没有 '#' 标志，则不应出现基数字符。低位数字应以实现定义的方式四舍五入。E 转换说明符应产生用 'E' 代替 'e' 引入指数的数字。指数应始终包含至少两位数字。如果值为零，则指数为零。

表示无穷大或 NaN 的 double 参数应以 f 或 F 转换说明符的形式转换。

**g, G** - 表示浮点数的 double 参数应根据转换的值和精度以 f 或 e 形式（或在 G 转换说明符的情况下以 F 或 E 形式）转换。设 P 等于精度（如果非零），如果精度省略则为 6，如果精度为零则为 1。那么，如果使用 E 形式的转换指数为 X：

- 如果 P>X>=-4，则转换使用 f（或 F）形式和精度 P-(X+1)。
- 否则，转换使用 e（或 E）形式和精度 P-1。

最后，除非使用 '#' 标志，否则结果的小数部分应删除任何尾随零，如果没有剩余的小数部分，则删除小数点字符。

表示无穷大或 NaN 的 double 参数应以 f 或 F 转换说明符的形式转换。

**a, A** - 表示浮点数的 double 参数应转换为 "[-]0x_h_.hhhh_p±d" 形式，其中小数点字符前有一个十六进制数字（如果参数是归一化浮点数，则该数字非零，否则未指定），其后的十六进制数字位数等于精度；如果精度缺失且 FLT_RADIX 是 2 的幂，则精度应足以精确表示该值；如果精度缺失且 FLT_RADIX 不是 2 的幂，则精度应足以区分 double 类型的值，但可以省略尾随零；如果精度为零且未指定 '#' 标志，则不应出现小数点字符。对于 a 转换使用字母 "abcdef"，对于 A 转换使用字母 "ABCDEF"。A 转换说明符产生用 'X' 和 'P' 代替 'x' 和 'p' 的数字。指数应始终包含至少一个数字，并且只包含表示 2 的十进制指数所需的更多数字。如果值为零，则指数为零。

表示无穷大或 NaN 的 double 参数应以 f 或 F 转换说明符的形式转换。

**c** - int 参数应转换为 unsigned char，并将结果字节写入。

如果存在 l（小写字母 l）限定符，则 wint_t 参数应转换为多字节序列，如同调用 wcrtomb() 并带有指向至少 MB_CUR_MAX 字节存储的指针、转换为 wchar_t 的 wint_t 参数和初始转换状态，并写入结果字节。

**s** - 参数应是指向 char 数组的指针。应写入数组中的字节直到（但不包括）任何终止空字节。如果指定了精度，则写入的字节数不应超过该数量。如果未指定精度或精度大于数组大小，则应用程序应确保数组包含空字节。

如果存在 l（小写字母 l）限定符，则参数应是指向 wchar_t 类型数组的指针。数组中的宽字符应转换为字符（每个如同调用 wcrtomb() 函数，转换状态由在第一个宽字符转换前初始化为零的 mbstate_t 对象描述）直到包括终止空宽字符。应写入结果字符直到（但不包括）终止空字符（字节）。如果未指定精度，则应用程序应确保数组包含空宽字符。如果指定了精度，则写入的字符（字节）数不应超过该数量（包括任何移位序列，如果有），并且如果为等于精度给出的字符序列长度，函数需要访问数组末尾之后的一个宽字符，则数组应包含空宽字符。在任何情况下都不应写入部分字符。

**p** - 参数应是指向 void 的指针。指针的值以实现定义的方式转换为可打印字符序列。

**n** - 参数应是指向整数的指针，其中写入此次调用 fprintf() 函数之一到目前为止写入输出的字节数。不转换任何参数。

**C** - 等同于 lc。

**S** - 等同于 ls。

**%** - 写入 '%' 字符；不应转换任何参数。应用程序应确保完整的转换规范为 %%。

如果转换规范与上述形式之一不匹配，则行为未定义。如果任何参数不是相应转换规范的正确类型，则行为未定义。

在任何情况下，不存在或过小的字段宽度都不会导致字段截断；如果转换结果宽于字段宽度，则应扩展字段以包含转换结果。由 fprintf() 和 printf() 生成的字符被打印如同调用了 fputc()。

对于 a 和 A 转换说明符，如果 FLT_RADIX 是 2 的幂，则值应正确四舍五入为具有给定精度的十六进制浮点数。

对于 a 和 A 转换，如果 FLT_RADIX 不是 2 的幂且结果不能以给定精度精确表示，则结果应为给定精度的十六进制浮点样式中的两个相邻数字之一，附加要求是误差对于当前舍入方向应具有正确的符号。

对于 e、E、f、F、g 和 G 转换说明符，如果有效十进制数字的数量最多为 DECIMAL_DIG，则结果应正确四舍五入。如果有效十进制数字的数量超过 DECIMAL_DIG 但源值可以用 DECIMAL_DIG 位数字精确表示，则结果应为带有尾随零的精确表示。否则，源值由两个相邻的十进制字符串 L < U 限定，两者都具有 DECIMAL_DIG 位有效数字；结果十进制字符串 D 的值应满足 L <= D <= U，附加要求是误差对于当前舍入方向应具有正确的符号。

文件的最后数据修改时间和最后文件状态更改时间戳应标记为更新：

1. 在调用成功执行 fprintf() 或 printf() 与下一次在同一流上成功完成调用 fflush() 或 fclose() 或调用 exit() 或 abort() 之间
2. 成功完成调用 dprintf() 时

## 返回值

成功完成时，dprintf()、fprintf() 和 printf() 函数应返回传输的字节数。

成功完成时，asprintf() 函数应返回写入存储在 ptr 引用位置的分配字符串的字节数，不包括终止空字节。

成功完成时，sprintf() 函数应返回写入 s 的字节数，不包括终止空字节。

成功完成时，snprintf() 函数应返回如果 n 足够大则将写入 s 的字节数，不包括终止空字节。

如果遇到错误，这些函数应返回负值并设置 errno 以指示错误。对于 asprintf()，如果无法分配内存，或者发生其他错误，则函数应返回负值，ptr 引用的位置的内容未定义，但不应引用已分配的内存。

如果在调用 snprintf() 时 n 的值为零，则不应写入任何内容，应返回如果 n 足够大则将写入的字节数（不包括终止空字节），且 s 可能为空指针。

## 错误

有关 dprintf()、fprintf() 和 printf() 失败和可能失败的条件，请参考 fputc() 或 fputwc()。

此外，所有形式的 fprintf() 在以下情况下应失败：

- **[EILSEQ]** - 检测到与有效字符不对应的宽字符代码。
- **[EOVERFLOW]** - 要返回的值大于 {INT_MAX}。

asprintf() 函数在以下情况下应失败：

- **[ENOMEM]** - 可用存储空间不足。

dprintf() 函数在以下情况下可能失败：

- **[EBADF]** - fildes 参数不是有效的文件描述符。

dprintf()、fprintf() 和 printf() 函数在以下情况下可能失败：

- **[ENOMEM]** - 可用存储空间不足。

## 示例

### 打印语言无关的日期和时间

以下语句可用于使用语言无关的格式打印日期和时间：

```c
printf(format, weekday, month, day, hour, min);
```

对于美国用法，format 可以是指向以下字符串的指针：

```c
"%s, %s %d, %d:%.2d\n"
```

此示例将产生以下消息：

```
Sunday, July 3, 10:02
```

对于德国用法，format 可以是指向以下字符串的指针：

```c
"%1$s, %3$d. %2$s, %4$d:%5$.2d\n"
```

此 format 定义将产生以下消息：

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

### 打印使用信息

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
// log2(10) = 约等于 10/33
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
wchar_t wz [3] = L"@@";       // 以零终止
wchar_t wn [3] = L"@@";      // 未终止

fprintf (stdout,"%ls", wz);   // 输出 6 字节
fprintf (stdout,"%ls", wn);   // 未定义，因为 wn 没有终止符
fprintf (stdout,"%4ls", wz);  // 输出 3 字节
fprintf (stdout,"%4ls", wn);  // 输出 3 字节；不需要终止符
fprintf (stdout,"%9ls", wz);  // 输出 6 字节
fprintf (stdout,"%9ls", wn);  // 输出 9 字节；不需要终止符
fprintf (stdout,"%10ls", wz); // 输出 6 字节
fprintf (stdout,"%10ls", wn); // 未定义，因为 wn 没有终止符
```

在示例的最后一行，处理三个字符后，已输出九个字节。然后必须检查第四个字符以确定它转换为一个还是多个字节。如果它转换为多个字节，则输出只有九个字节。由于数组中没有第四个字符，因此行为未定义。

## 应用程序用法

如果调用 fprintf() 的应用程序具有任何 wint_t 或 wchar_t 类型的对象，则它还必须包含 <wchar.h> 头文件以定义这些对象。

成功调用 asprintf() 分配的空间应随后通过调用 free() 释放。

## 基本原理

如果实现检测到格式没有足够的参数，建议函数应失败并报告 [EINVAL] 错误。

为 %lc 转换指定的行为与 ISO C 标准中的规范略有不同，因为打印空宽字符产生空字节而不是 0 字节输出，这是严格按照 ISO C 标准指示表现得像将 %ls 说明符应用于第一个元素是空宽字符的 wchar_t 数组所要求的。为每个可能的宽字符（包括空字符）要求多字节输出符合历史实践，并与 fprintf() 中的 %c 以及 fwprintf() 中的 %c 和 %lc 保持一致。预计 ISO C 标准的未来版本将更改以匹配此处指定的行为。

## 未来方向

无。

## 另请参阅

- 2.5 标准 I/O 流
- fputc()
- fscanf()
- setlocale()
- strfmon()
- strlcat()
- wcrtomb()
- wcslcat()

XBD：
- 7. 语言环境
- <inttypes.h>
- <stdio.h>
- <wchar.h>

## 更改历史

首次发布于 Issue 1。源自 SVID 的 Issue 1。

### Issue 5

与 ISO/IEC 9899:1990/Amendment 1:1995 (E) 对齐。具体来说，l（小写字母 l）限定符现在可以与 c 和 s 转换说明符一起使用。

snprintf() 函数是 Issue 5 中的新增内容。

### Issue 6

超出 ISO C 标准的扩展被标记。

规范性文本更新，避免对应用程序要求使用术语"必须"。

为与 ISO/IEC 9899:1999 标准对齐进行以下更改：

- fprintf()、printf()、snprintf() 和 sprintf() 的原型更新，并从 snprintf() 中移除了 XSI 底纹。
- snprintf() 的描述与 ISO C 标准对齐。请注意，这取代了 The Open Group Base Resolution bwg98-006 中的 snprintf() 描述，该描述改变了 Issue 5 的行为。
- 描述部分更新。

描述部分更新为一致使用"转换说明符"和"转换规范"术语。

整合 ISO/IEC 9899:1999 标准，技术勘误 1。

添加打印宽字符的示例。

### Issue 7

应用 Austin Group 解释 1003.1-2001 #161，更新 0 标志的描述。

应用 Austin Group 解释 1003.1-2001 #170。

应用 ISO/IEC 9899:1999 标准，技术勘误 2 #68 (SD5-XSH-ERN-70)，修订 g 和 G 的描述。

应用 SD5-XSH-ERN-174。

从 The Open Group 技术标准 2006，扩展 API 集第 1 部分添加 dprintf() 函数。

与 %n$ 形式转换规范和撇号标志相关的功能从 XSI 选项移至基础。

进行与支持细粒度时间戳相关的更改。

应用 POSIX.1-2008，技术勘误 1，XSH/TC1-2008/0163 [302]、XSH/TC1-2008/0164 [316]、XSH/TC1-2008/0165 [316]、XSH/TC1-2008/0166 [451,291] 和 XSH/TC1-2008/0167 [14]。

应用 POSIX.1-2008，技术勘误 2，XSH/TC2-2008/0126 [894]、XSH/TC2-2008/0127 [557] 和 XSH/TC2-2008/0128 [936]。

### Issue 8

应用 Austin Group 缺陷 986，将 strlcat() 和 wcslcat() 添加到"另请参阅"部分。

应用 Austin Group 缺陷 1020，澄清 snprintf() 参数 n 限制写入 s 的字节数；它不一定与 s 的大小相同。

应用 Austin Group 缺陷 1021，将"返回值"部分中的"输出错误"更改为"错误"。

应用 Austin Group 缺陷 1137，澄清在转换规范中使用 "%n$" 和 "*m$"。

应用 Austin Group 缺陷 1205，更改 % 转换说明符的描述。

应用 Austin Group 缺陷 1219，移除 snprintf() 特有的 [EOVERFLOW] 错误。

应用 Austin Group 缺陷 1496，添加 asprintf() 函数。

应用 Austin Group 缺陷 1562，澄清应用程序有责任确保格式是一个字符串，如果存在初始转换状态，则在该状态下开始和结束。

应用 Austin Group 缺陷 1647，更改 c 转换说明符的描述，并更新本卷 POSIX.1-2024 遵从 ISO C 标准的陈述，以便在传入空宽字符时排除 %lc 转换。