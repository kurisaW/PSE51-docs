# strtok, strtok_r — 将字符串分割为标记

## 概要

```c
#include <string.h>

char *strtok(char *restrict s, const char *restrict sep);

[X] char *strtok_r(char *restrict s, const char *restrict sep,
                  char **restrict state);
```

## 描述

对于 `strtok()`：
[X] 本参考页描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是非故意的。本 POSIX.1-2024 卷遵循 ISO C 标准。

一系列对 `strtok()` 的调用将 `s` 指向的字符串分解为一系列标记（token），每个标记由 `sep` 指向的字符串中的一个字节分隔。序列中的第一次调用以 `s` 作为第一个参数，后续调用以空指针作为第一个参数。`sep` 指向的分隔符字符串在每次调用中可以不同。

序列中的第一次调用在 `s` 指向的字符串中搜索第一个**不**包含在当前 `sep` 指向的分隔符字符串中的字节。如果找不到这样的字节，那么 `s` 指向的字符串中没有标记，`strtok()` 应返回空指针。如果找到这样的字节，它就是第一个标记的开始。

`strtok()` 函数然后从该位置开始搜索包含在当前分隔符字符串中的字节。如果找不到这样的字节，当前标记延伸到 `s` 指向的字符串的末尾，后续对标记的搜索应返回空指针。如果找到这样的字节，它会被 NUL 字符覆盖，从而终止当前标记。`strtok()` 函数保存指向下一个字节的指针，下一次搜索标记将从此开始。

每个后续调用（以空指针作为第一个参数的值）都从保存的指针开始搜索，并如上所述行为。

实现的行为应如同本 POSIX.1-2024 卷中定义的任何函数都不会调用 `strtok()`。

`strtok()` 函数不需要是线程安全的；但是，`strtok()` 应避免与所有其他函数发生数据竞争。

[X] `strtok_r()` 函数应等同于 `strtok()`，不同之处在于 `strtok_r()` 应是线程安全的，并且参数 `state` 指向一个用户提供的指针，允许 `strtok_r()` 在扫描同一字符串的调用之间维护状态。应用程序应确保 `state` 指向的指针对于由 `strtok_r()` 调用并发处理的每个字符串（`s`）是唯一的。应用程序不需要将 `state` 指向的指针初始化为任何特定值。实现不应将 `state` 指向的指针更新为指向（直接或间接）字符串 `s` 之外的、需要调用者释放或释放的资源。

[X] 在有效输入上，`strtok()` 和 `strtok_r()` 函数不应更改 `errno` 的设置。

## 返回值

成功完成后，`strtok()` 应返回指向标记第一个字节的指针。否则，如果没有标记，`strtok()` 应返回空指针。

[X] `strtok_r()` 函数应返回指向找到的标记的指针，当找不到标记时返回空指针。

## 错误

未定义错误。

## 示例

### 搜索单词分隔符

以下示例搜索由空格字符分隔的标记。

```c
#include <string.h>
...
char *token;
char line[] = "LINE TO BE SEPARATED";
char *search = " ";

/* Token 将指向 "LINE"。 */
token = strtok(line, search);

/* Token 将指向 "TO"。 */
token = strtok(NULL, search);
```

### 在缓冲区中查找前两个字段

以下示例使用 `strtok()` 查找由空格、制表符或换行符的任意组合分隔的两个字符串（一个键和与该键关联的数据），这些分隔符位于 `buffer` 指向的字符数组的开始处。

```c
#include <string.h>
...
char *buffer;
...
struct element {
    char *key;
    char *data;
} e;
...
// 加载缓冲区...
...
// 获取键及其数据...
e.key = strtok(buffer, " \t\n");
e.data = strtok(NULL, " \t\n");
// 处理缓冲区的其余内容...
...
```

## 应用程序用法

请注意，如果 `sep` 是空字符串，`strtok()` 和 `strtok_r()` 会返回指向正在被标记化的字符串剩余部分的指针。

`strtok_r()` 函数是线程安全的，并将其状态存储在用户提供的缓冲区中，而不是可能使用静态数据区域，该区域可能被来自另一个线程的不相关调用覆盖。

## 基本原理

`strtok()` 函数在较大的字符串中搜索分隔符字符串。它返回指向分隔符字符串之间最后一个子字符串的指针。此函数使用静态存储来跟踪调用之间的当前字符串位置。新函数 `strtok_r()` 接受一个额外的参数 `state`，用于跟踪字符串中的当前位置。

## 未来方向

无。

## 另请参阅

XBD `<string.h>`

## 变更历史

首次在 Issue 1 中发布。派生自 SVID 的 Issue 1。

### Issue 5

包含 `strtok_r()` 函数以与 POSIX 线程扩展对齐。

在描述中添加了一个注释，表明 `strtok()` 函数不需要是可重入的。

### Issue 6

标记了超出 ISO C 标准的扩展。

`strtok_r()` 函数被标记为线程安全函数选项的一部分。

在描述中，关于可重入性的注释被扩展以涵盖线程安全性。

应用程序用法部分被更新，包含关于线程安全函数及其避免可能使用静态数据区域的注释。

为了与 ISO/IEC 9899:1999 标准对齐，向 `strtok()` 和 `strtok_r()` 原型添加了 `restrict` 关键字。

### Issue 7

应用了 Austin Group Interpretation 1003.1-2001 #156。

应用了 SD5-XSH-ERN-235，更正了一个示例。

`strtok_r()` 函数从线程安全函数选项移动到基本规范。

应用了 POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0615 [177]。

应用了 POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0350 [878]。

### Issue 8

应用了 Austin Group Defect 448，添加了 `strtok()` 和 `strtok_r()` 在有效输入上不更改 `errno` 设置的要求。

应用了 Austin Group Defect 1302，使 `strtok()` 函数与 ISO/IEC 9899:2018 标准对齐。