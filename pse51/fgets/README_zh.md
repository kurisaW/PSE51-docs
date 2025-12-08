# fgets

## 概要

```c
#include <stdio.h>

char *fgets(char *restrict s, int n, FILE *restrict stream);
```

## 描述

本参考页描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是非故意的。本卷 POSIX.1-2024 遵从 ISO C 标准。

`fgets()` 函数应从 `stream` 中读取字节到由 `s` 指向的数组中，直到读取了 `n`-1 个字节，或读取了 `<newline>`（换行符）并传输到 `s`，或遇到文件结束条件。应在读取到数组中的最后一个字节之后立即写入一个空字节。如果在读取任何字节之前遇到文件结束条件，则由 `s` 指向的数组内容不应更改。

`fgets()` 函数可能会标记与 `stream` 关联的文件的最后数据访问时间戳以供更新。第一次成功执行使用 `stream` 并返回非先前调用 `ungetc()` 所提供数据的 `fgetc()`、`fgets()`、`fread()`、`fscanf()`、`getc()`、`getchar()`、`getdelim()`、`getline()` 或 `scanf()` 时，应标记最后数据访问时间戳以供更新。

## 返回值

成功完成后，`fgets()` 应返回 `s`。如果流位于文件结尾，应设置流的文件结束指示器，`fgets()` 应返回空指针。如果发生错误，应设置流的错误指示器，`fgets()` 应返回空指针，并应设置 `errno` 以指示错误。

## 错误

参考 `fgetc()`。

## 示例

### 读取输入

以下示例使用 `fgets()` 读取输入行。它假定正在读取的文件是文本文件，并且该文本文件中的行长度不超过 16384 字节（或在运行的实现上，如果 {LINE_MAX} 小于 16384，则为 {LINE_MAX}）。（注意，如果 `sysconf(_SC_LINE_MAX)` 返回 -1 且未设置 `errno`，则标准工具没有行长度限制。此示例假设 `sysconf(_SC_LINE_MAX)` 不会失败。）

```c
#include <limits.h>
#include <stdio.h>
#include <unistd.h>

#define MYLIMIT 16384

char *line;
int line_max;

if (LINE_MAX >= MYLIMIT) {
    /* 使用 MYLIMIT 的最大行大小。如果 LINE_MAX
       大于我们的限制，sysconf() 无法报告
       更小的限制。 */
    line_max = MYLIMIT;
} else {
    long limit = sysconf(_SC_LINE_MAX);
    line_max = (limit < 0 || limit > MYLIMIT) ? MYLIMIT : (int)limit;
}

/* line_max + 1 为 fgets() 添加的空字节留出空间。 */
line = malloc(line_max + 1);
if (line == NULL) {
    /* 空间不足 */
    ...
    return error;
}

while (fgets(line, line_max + 1, fp) != NULL) {
    /* 验证是否已读取完整行... */
    /* 如果没有，报告错误或准备将下一次循环
       视为读取当前行的继续。 */
    ...
    /* 处理行... */
    ...
}
free(line);
...
```

## 应用用法

无。

## 原理

无。

## 未来方向

无。

## 另请参见

- [2.5 标准 I/O 流](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_05)
- [`fgetc()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fgetc.html)
- [`fopen()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fopen.html)
- [`fread()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fread.html)
- [`fscanf()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fscanf.html)
- [`getc()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/getc.html)
- [`getchar()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/getchar.html)
- [`getdelim()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/getdelim.html)
- [`ungetc()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/ungetc.html)
- XBD [`<stdio.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdio.h.html)

## 变更历史

### 首次发布于 Issue 1。源自 SVID 的 Issue 1。

### Issue 6

标记了超出 ISO C 标准的扩展。

`fgets()` 的原型已更改以与 ISO/IEC 9899:1999 标准保持一致。

### Issue 7

应用了 Austin Group Interpretation 1003.1-2001 #051，更新了标记最后数据访问时间戳以供更新的函数列表。

应用了 POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0134 [182] 和 XSH/TC1-2008/0135 [14]。

应用了 POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0114 [468]。

### Issue 8

应用了 Austin Group Defect 1330，移除了过时的接口。

应用了 Austin Group Defect 1624，更改了 RETURN VALUE 部分。

---
