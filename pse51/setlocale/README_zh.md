# setlocale — 设置程序本地化环境

## 概要

```c
#include <locale.h>

char *setlocale(int category, const char *locale);
```

## 描述

`setlocale()` 函数根据 `category` 和 `locale` 参数选择全局本地化环境的适当部分，可用于更改或查询整个全局本地化环境或其部分内容。对于 `category`，值 `LC_ALL` 表示整个全局本地化环境；其他值仅表示全局本地化环境的一部分：

- **LC_COLLATE**
  影响正则表达式和排序函数的行为。

- **LC_CTYPE**
  影响正则表达式、字符分类、字符转换函数和宽字符函数的行为。

- **LC_MESSAGES**
  影响 `nl_langinfo()` 返回的肯定和否定响应表达式以及消息目录的定位方式。它也可能影响返回或写入消息字符串的函数的行为。

- **LC_MONETARY**
  影响处理货币值的函数的行为。

- **LC_NUMERIC**
  影响处理数值的函数的行为。

- **LC_TIME**
  影响时间转换函数的行为。

`locale` 参数是一个指向字符字符串的指针，该字符串包含 `category` 的所需设置。此字符串的内容由实现定义。此外，为 `category` 的所有设置定义了以下预设的 `locale` 值：

- **"POSIX"**
  指定用于 C 语言翻译的最小环境，称为 POSIX 本地化环境。POSIX 本地化环境是在进入 `main()` 时的默认全局本地化环境。

- **"C"**
  等同于 "POSIX"。

- **""**
  指定由实现定义的本机环境。指定类别的新本地化环境名称的确定取决于相关环境变量 `LC_*` 和 `LANG` 的值；参见 POSIX.1-2024 基础定义卷。

- **空指针**
  指示 `setlocale()` 查询当前全局本地化环境设置，如果 `category` 不是 `LC_ALL`，则返回本地化环境的名称；如果 `category` 是 `LC_ALL`，则返回一个编码所有单个类别本地化环境名称的字符串。

设置全局本地化环境的所有类别类似于连续设置全局本地化环境的每个单独类别，不同之处在于所有错误检查都在执行任何操作之前完成。要设置全局本地化环境的所有类别，可以按以下方式调用 `setlocale()`：

```c
setlocale(LC_ALL, "");
```

在这种情况下，`setlocale()` 应首先根据优先级规则验证它需要的所有环境变量的值是否表示支持的本地化环境。如果这些环境变量搜索中的任何一个产生的本地化环境不受支持（且非空），`setlocale()` 应返回空指针且全局本地化环境不应更改。如果所有环境变量都命名了支持的本地化环境，`setlocale()` 应继续执行，就好像已为每个类别调用过它一样，使用相关环境变量的适当值，如果没有这样的值，则使用实现定义的默认值。

使用 `setlocale()` 建立的全局本地化环境仅应用于那些没有使用 `uselocale()` 设置当前本地化环境的线程，或者其当前本地化环境已使用 `uselocale(LC_GLOBAL_LOCALE)` 设置为全局本地化环境的线程。

实现的行为应如同 POSIX.1-2024 卷中定义的任何函数都不调用 `setlocale()`。

`setlocale()` 函数不需要是线程安全的；但是，它应避免与所有不影响且不受全局本地化环境影响的函数调用发生数据竞争。

## 返回值

成功完成时，`setlocale()` 应返回与新本地化环境的指定类别相关联的字符串。否则，`setlocale()` 应返回空指针且全局本地化环境不应更改。

`locale` 的空指针将导致 `setlocale()` 返回一个指向与当前全局本地化环境的指定 `category` 相关联的字符串的指针。全局本地化环境不应更改。

`setlocale()` 返回的字符串应使得使用该字符串及其关联的 `category` 的后续调用应恢复全局本地化环境的该部分。应用程序不应修改返回的字符串。返回的字符串指针可能会失效，或者字符串内容可能会被后续的 `setlocale()` 调用覆盖。如果调用线程终止，返回的指针也可能失效。

## 错误

未定义错误。

## 应用程序用法

以下代码说明程序如何为一种语言初始化国际化环境，同时选择性地修改全局本地化环境，使得正则表达式和字符串操作可以应用于以不同语言记录的文本：

```c
setlocale(LC_ALL, "De");
setlocale(LC_COLLATE, "Fr@dict");
```

国际化程序可以通过按以下方式调用 `setlocale()` 来根据环境变量设置启动语言操作：

```c
setlocale(LC_ALL, "");
```

更改 `LC_MESSAGES` 的设置对已通过 `catopen()` 调用打开的目录没有影响。

为了在多线程运行时使用不同的本地化环境设置，应用程序应优先使用 `uselocale()` 而不是 `setlocale()`。

## 原理

下文中对国际化环境或本地化环境的引用涉及进程的全局本地化环境。这可以使用 `uselocale()` 为单个线程覆盖。

ISO C 标准定义了一组支持国际化的函数。这些函数最重要的方面之一是设置和查询*国际化环境*的设施。国际化环境是影响某些功能行为的信息存储库，即：

1. 字符处理
2. 排序
3. 日期/时间格式化
4. 数值编辑
5. 货币格式化
6. 消息传递

`setlocale()` 函数为应用程序开发人员提供了设置国际化环境的全部或部分（称为*类别*）的能力。这些类别对应于上述功能区域。

`setlocale()` 有两个主要用途：

1. 查询国际化环境以了解其设置内容
2. 将国际化环境或*本地化环境*设置为特定值

要查询国际化环境，使用特定类别和空指针作为本地化环境来调用 `setlocale()`。空指针是 `setlocale()` 的特殊指令，告诉它查询而不是设置国际化环境。

有三种方式使用 `setlocale()` 设置国际化环境：

- `setlocale(category, string)`
  此用法将国际化环境中的特定 `category` 设置为对应于 `string` 值的特定值。例如：
  ```c
  setlocale(LC_ALL, "fr_FR.ISO-8859-1");
  ```

- `setlocale(category, "C")`
  ISO C 标准规定所有符合实现的实现上必须存在一个本地化环境。本地化环境的名称是 C，对应于支持 C 编程语言所需的最小国际化环境。

- `setlocale(category, "")`
  这将特定类别设置为实现定义的默认值。这对应于环境变量的值。

## 参见

`catopen()`, `exec`, `fprintf()`, `fscanf()`, `getlocalename_l()`, `isalnum()`, `isalpha()`, `isblank()`, `iscntrl()`, `isdigit()`, `isgraph()`, `islower()`, `isprint()`, `ispunct()`, `isspace()`, `isupper()`, `iswalnum()`, `iswalpha()`, `iswblank()`, `iswcntrl()`, `iswctype()`, `iswdigit()`, `iswgraph()`, `iswlower()`, `iswprint()`, `iswpunct()`, `iswspace()`, `iswupper()`, `iswxdigit()`, `isxdigit()`, `localeconv()`, `mblen()`, `mbstowcs()`, `mbtowc()`, `newlocale()`, `nl_langinfo()`, `perror()`, `psiginfo()`, `strcoll()`, `strerror()`, `strfmon()`, `strsignal()`, `strtod()`, `strxfrm()`, `tolower()`, `toupper()`, `towlower()`, `towupper()`, `uselocale()`, `wcscoll()`, `wcstod()`, `wcstombs()`, `wcsxfrm()`, `wctomb()`

XBD 7. 本地化环境，8. 环境变量，`<langinfo.h>`，`<locale.h>`

## 更改历史

首次发布于第 3 期。在后续期中更新，以与 POSIX 线程扩展对齐，标记超出 ISO C 标准的扩展，并应用各种技术更正。
