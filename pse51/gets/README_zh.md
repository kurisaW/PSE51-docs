# gets

## 概要

```c
#include <stdio.h>

char *gets(char *s);
```

## 描述

本参考页描述的功能与 ISO C 标准保持一致。如果此处描述的要求与 ISO C 标准之间存在冲突，则非有意为之。POSIX.1-2017 卷遵循 ISO C 标准。

`gets()` 函数应从标准输入流 `stdin` 读取字节到 `s` 指向的数组中，直到读取到 `<newline>`（换行符）或遇到文件结束条件。任何 `<newline>` 都将被丢弃，并在读取到数组中的最后一个字节之后立即放置一个空字节。

`gets()` 函数可以标记与流关联的文件的最后数据访问时间戳以进行更新。第一次成功执行使用 `stream` 的 `fgetc()`、`fgets()`、`fread()`、`fscanf()`、`getc()`、`getchar()`、`getdelim()`、`getline()`、`gets()` 或 `scanf()` 函数时，应标记最后数据访问时间戳以进行更新，这些函数返回的数据不是由先前调用 `ungetc()` 提供的。

## 返回值

成功完成时，`gets()` 应返回 `s`。如果流的文件结束指示符已设置，或者流处于文件结���位置，则应设置流的文件结束指示符并且 `gets()` 应返回空指针。如果发生读取错误，则应设置流的错误指示符，`gets()` 应返回空指针，并设置 `errno` 以指示错误。

## 错误

参考 [`fgetc()`](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fgetc.html)。

---

*以下部分为补充信息。*

## 示例

无。

## 应用程序使用

读取溢出 `s` 指向的数组的行会导致未定义行为。建议使用 [`fgets()`](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fgets.html)。

由于用户无法指定传递给 `gets()` 的缓冲区长度，不鼓励使用此函数。读取的字符串长度是无限的。可能以导致应用程序失败或可能的系统安全违规的方式溢出此缓冲区。

应用程序应使用 [`fgets()`](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fgets.html) 函数而不是已过时的 `gets()` 函数。

## 原理

标准开发者决定将 `gets()` 函数标记为过时，即使它在 ISO C 标准中，这是由于缓冲区溢出的可能性。

## 未来方向

`gets()` 函数可能在未来的版本中被移除。

## 参见

- [标准 I/O 流](https://pubs.opengroup.org/onlinepubs/9699919799/functions/V2_chap02.html#tag_15_05)
- [`feof()`](https://pubs.opengroup.org/onlinepubs/9699919799/functions/feof.html)
- [`ferror()`](https://pubs.opengroup.org/onlinepubs/9699919799/functions/ferror.html)
- [`fgets()`](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fgets.html)
- [<stdio.h>](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/stdio.h.html)

## 更改历史

首次发布于 Issue 1。源自 SVID 的 Issue 1。

### Issue 6

标记了超出 ISO C 标准的扩展。

### Issue 7

应用了 Austin Group Interpretation 1003.1-2001 #051，阐明了返回值部分。

`gets()` 函数被标记为过时。

进行了与细粒度时间戳支持相关的更改。

应用了 POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0257 [14]。

*补充信息结束。*

---
