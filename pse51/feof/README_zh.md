# feof

The Open Group Base Specifications Issue 8
IEEE Std 1003.1-2024
Copyright © 2001-2024 The IEEE and The Open Group

---

## 名称 (NAME)

`feof` — 测试流的文件结束指示器

## 概要 (SYNOPSIS)

```c
#include <stdio.h>

int feof(FILE *stream);
```

## 描述 (DESCRIPTION)

[CX] 本参考页描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是非故意的。POSIX.1-2024 本卷遵循 ISO C 标准。

`feof()` 函数应测试由 `stream` 指向的流的文件结束指示器。

[CX] 如果 `stream` 有效，`feof()` 函数不应改变 `errno` 的设置。

## 返回值 (RETURN VALUE)

当且仅当为 `stream` 设置了文件结束指示器时，`feof()` 函数应返回非零值。

## 错误 (ERRORS)

未定义错误。

---

*以下章节为参考信息。*

## 示例 (EXAMPLES)

无。

## 应用程序用法 (APPLICATION USAGE)

无。

## 原理 (RATIONALE)

无。

## 未来方向 (FUTURE DIRECTIONS)

无。

## 另请参见 (SEE ALSO)

- [`clearerr()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/clearerr.html)
- [`ferror()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/ferror.html)
- [`fopen()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fopen.html)

XBD [`<stdio.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdio.h.html)

## 变更历史 (CHANGE HISTORY)

首次在 Issue 1 中发布。源自 SVID 的 Issue 1。

### Issue 7

应用了 POSIX.1-2008、技术勘误 1、XSH/TC1-2008/0124 [401]。

